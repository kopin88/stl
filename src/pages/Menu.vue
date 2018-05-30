<template>
  <v-ons-page modifier="white">
    <div class="profile-pic">
      <img src="../assets/stl-logo.png">
    </div>

    <!-- <v-ons-list-title>Access</v-ons-list-title> -->
    <v-ons-list>
      <v-ons-list-item v-for="(item, index) in access" :key="item.title"
        :modifier="md && 'nodivider'"
        @click="loadView(index)"
      >
        <div class="left">
          <v-ons-icon fixed-width class="list-item__icon" :icon="item.icon"></v-ons-icon>
        </div>
        <div class="center">
          {{ item.title }}
        </div>
        <div class="right">
          <v-ons-icon icon="ion-chevron-right"></v-ons-icon>
        </div>
      </v-ons-list-item>
    </v-ons-list>

    <v-ons-list-title>Setting</v-ons-list-title>
    <v-ons-list>
      <v-ons-list-item v-if="authState.api_token && authState.user_id === 1" v-for="page of pages" :key="page.title"
        @click="push(page.component, page.title)"
        :modifier="md && 'nodivider'"
      >
        <div class="left">
          <v-ons-icon fixed-width class="list-item__icon" :icon="page.icon"></v-ons-icon>
        </div>
        <div class="center">
          {{ page.title }}
        </div>
        <div class="right">
          <v-ons-icon icon="ion-chevron-right"></v-ons-icon>
        </div>
      </v-ons-list-item>
      <v-ons-list-item @click="logout" :modifier="md && 'nodivider'">
        <div class="left">
          <v-ons-icon fixed-width class="list-item__icon" icon="ion-locked, material:md-lock"></v-ons-icon>
        </div>
        <div class="center">
          Log Out
        </div>
        <div class="right">
          <v-ons-icon icon="ion-chevron-right"></v-ons-icon>
        </div>
      </v-ons-list-item>

    </v-ons-list>
  </v-ons-page>
</template>

<script>
import User from '../views/Auth/User.vue'
import { post, apiDomain } from '../helpers/api'
import Auth from '../store/auth'
export default {
  methods: {
    loadView(index) {
      this.$store.commit('tabbar/set', index);
      this.$store.commit('splitter/toggle');
    },
    loadLink(url) {
      window.open(url, '_blank');
    },
    push(page, key) {
      this.$store.commit('navigator/push', {
        extends: page,
        data() {
          return {
            toolbarInfo: {
              backLabel: 'Home',
              title: key
            }
          }
        }
      });
    },
		logout() {
			post( apiDomain + '/logout')
			    .then((res) => {
			        if(res.data.done) {
			            // remove token
			            Auth.remove()
                  location.reload(true);
			        }
			    })
		}
  },
  data() {
    return {
      authState: Auth.state,
      pages: [
        {
          component: User,
          title: 'User',
          icon: 'ion-person-add, material:md-person-add',
        }
      ],
      access: [
        // {
        //   title: 'Home',
        //   icon: 'ion-home, material:md-home'
        // },
        {
          title: 'ကုန္ပစၥည္း စာရင္း',
          icon: 'ion-clipboard, material:md-assignment'
        },
        {
          title: 'ေငြစာရင္း',
          icon: 'ion-document-text, material:md-document-text'
        },
        {
          title: 'လုပ္ေဆာင္ရန္',
          icon: 'ion-flag, material: md-flag'
        }
      ]
    };
  }
};
</script>

<style scoped>
.profile-pic {
  width: 100%;
  background-color: #fff;
  border-bottom: 1px solid #DDD;
  color: rgba(0, 0, 0, .56);
  padding: 30px 30px 8px;
}
.page--material .profile-pic {
  background-color: #f6f6f6;
}

.profile-pic > img {
  display: block;
  max-width: 100%;
}
</style>

<style>
.page--material__background.page--white__background {
  background-color: #fff;
}
</style>
