<template>
  <v-ons-card>
      <v-ons-row width="100%">
        <v-ons-col class="text-center" width="15%">
            Show
        </v-ons-col>
        <v-ons-col width="20%">
          <select class="form-control" v-model="params.per_page" @change="fetchData" style="height:30px">
            <option value="10">10</option>
            <option value="25">25</option>
            <option value="50">50</option>
          </select>
        </v-ons-col>
        <v-ons-col class="text-center" width="20%">
          entries
        </v-ons-col>
        <v-ons-col width="5%">
        </v-ons-col>
        <v-ons-col width="20%">
          <button @click="prev" class="btn btn-success btn-round btn-sm">Prev</button>
        </v-ons-col>
        <v-ons-col width="20%">
          <button @click="next" class="btn btn-success btn-round btn-sm">Next</button>
        </v-ons-col>
      </v-ons-row>
      <v-ons-row style="margin-top:20px">
        <div class="table-responsive">
          <table class="table table-striped table-no-bordered table-hover dataTable dtr-inline" cellspacing="0" width="100%" style="width: 100%;" role="grid" aria-describedby="datatables_info">
            <thead>
              <tr role="row">
                <th v-for="item in thead">
                  <div class="dataviewer-th sorting" @click="sort(item.key)" v-if="item.sort">
                    <span>{{item.title}}</span>
                    <span v-if="params.column === item.key">
                      <span v-if="params.direction === 'asc'">&#8593;</span>
                      <span v-else>&#8595;</span>
                    </span>
                  </div>
                  <div v-else>
                    <span>{{item.title}}</span>
                  </div>
                </th>
              </tr>
            </thead>
            <tfoot>
              <tr role="row">
                <td v-for="item in thead">
                  <span>{{item.title}}</span>
                </td>
              </tr>
            </tfoot>
            <tbody>
              <slot v-for="item in model.data" :item="item"></slot>
            </tbody>
          </table>
        </div>
      </v-ons-row>
      <v-ons-row width="100%">
        <v-ons-col class="text-center" width="15%">
          Show
        </v-ons-col>
        <v-ons-col class="text-center" width="33%">
          {{model.from}} - {{model.to}} of {{model.total}}
        </v-ons-col>
        <v-ons-col width=3%>
        </v-ons-col>
        <v-ons-col class="text-centert" width="12%">
          <small>page: </small>
        </v-ons-col>
        <v-ons-col class="text-center" width="15%">
          <input type="text" class="form-control" v-model="params.page" style="padding-top:0; padding-bottom:0;"
          @keyup.enter="fetchData">
        </v-ons-col>
        <v-ons-col class="text-center" width="20%">
            <small> of {{model.last_page}}</small>
        </v-ons-col>
      </v-ons-row>
  </v-ons-card>
</template>

<script>
    import Vue from 'vue'
    import Auth from '../store/auth'
    import axios from 'axios'

    export default {
        props: ['source', 'thead', 'filter', 'create', 'title'],
        data() {
            return {
    				    authState: Auth.state,
                showFilter: false,
                model: {
                    data: []
                },
                params: {
                    column: 'id',
                    direction: 'desc',
                    per_page: 10,
                    page: 1,
                    search_column: 'id',
                    search_operator: 'equal_to',
                    search_query_1: '',
                    search_query_2: ''
                },
                operators: {
                    equal_to: '=',
                    not_equal: '<>',
                    less_than: '<',
                    greater_than: '>',
                    less_than_or_equal_to: '<=',
                    greater_than_or_equal_to: '>=',
                    in: 'IN',
                    not_in: 'NOT IN',
                    like: 'LIKE',
                    between: 'BETWEEN'
                }
            }
        },
        beforeMount() {
            this.fetchData()
        },
        methods: {
            next() {
                if(this.model.next_page_url) {
                    this.params.page++
                    this.fetchData()
                }
            },
            prev() {
                if(this.model.prev_page_url) {
                    this.params.page--
                    this.fetchData()
                }
            },
            sort(column) {
                if(column === this.params.column) {
                    if(this.params.direction === 'desc') {
                        this.params.direction = 'asc'
                    } else {
                        this.params.direction = 'desc'
                    }
                } else {
                    this.params.column = column
                    this.params.direction = 'asc'
                }

                this.fetchData()
            },
            fetchData() {
                var vm = this
                axios.get(this.buildURL())
                    .then(function(response) {
                        Vue.set(vm.$data, 'model', response.data.model)
                    })
                    .catch(function(error) {
                        console.log(error)
                    })
            },
            buildURL() {
                var p = this.params
                return `${this.source}?column=${p.column}&direction=${p.direction}&per_page=${p.per_page}&page=${p.page}&search_column=${p.search_column}&search_operator=${p.search_operator}&search_query_1=${p.search_query_1}&search_query_2=${p.search_query_2}`
            }
        }
    }
</script>
