<template>
	<div class="image__preview" v-if="image">
		<img :src="image" class="image__preview">
		<button class="btn-img btn-img__danger image__close" @click="close">
			&times;
		</button>
	</div>
</template>
<script type="text/javascript">
	import {imgUrl} from '../helpers/api'
	export default {
		props: {
			forderPath:'',
			preview: {
			    type: [String, File],
			    default: null
			}
		},
		data() {
			return {
				image: null,
			}
		},
		created() {
			this.setPreview()
		},
		watch: {
			'preview': 'setPreview'
		},
		methods: {
			setPreview() {
				if(this.preview instanceof File ) {
					const fileReader = new FileReader()
					fileReader.onload = (event) => {
					  this.image = event.target.result
					}
					fileReader.readAsDataURL(this.preview)
				} else if (typeof this.preview === 'string') {
					// this.image = `/images/${this.preview}`
					this.image = imgUrl + this.forderPath + this.preview
				} else {
					this.image = null
				}
			},
			close() {
				this.$emit('close')
			}
		}
	}
</script>

<style scoped>

.image__preview {
         position: relative;
         /* height: 125px; */
         width: 100%;
   }
.image__close {
         position: absolute;
         right: 0;
         top: 0;
     }
</style>
