<template>
	<div class="image">
		<image-preview :forderPath="forderPath" :preview="value" @close="$emit('input', null)"
			 v-if="value"></image-preview>
		<div class="image__upload" v-else>
			<input
				style="display: none"
				type="file"
				accept="image/*"
				@change="upload"
				ref="fileInput">
			<button @click="$refs.fileInput.click()">Select Image</button>
		</div>
	</div>
</template>
<script type="text/javascript">
	import ImagePreview from './ImagePreview.vue'
	export default {
		components: {
			ImagePreview
		},
		props: {
			forderPath: '',
			value: {
			    type: [String, File],
			    default: null
			}
		},
		methods: {

			upload(e) {
				const files = e.target.files
				if (files && files.length > 0) {
				  this.$emit('input', files[0])
				}
			}
		}
	}
</script>

<style scoped>
.image__upload {
				 height: 200px;
				 /* width: 125px; */
         width: 200px;
				 background: #fafafa;
				 border: 1px dashed  rgba(0,0,0,.1);
				 display: flex;
				 justify-content: center;
				 align-items: center;
		 }

 input[type=file] {
   width: 90px;
 }
</style>
