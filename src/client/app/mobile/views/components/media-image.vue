<template>
<a class="mk-media-image" :href="image.url" target="_blank" :style="style" :title="image.name"></a>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
	props: {
		image: {
			type: Object,
			required: true
		},
		raw: {
			default: false
		}
	},
	computed: {
		lightmode(): boolean {
			return this.$store.state.device.lightmode;
		},
		style(): any {
			return {
				'background-color': this.image.properties.avgColor && this.image.properties.avgColor.length == 3 ? `rgb(${this.image.properties.avgColor.join(',')})` : 'transparent',
				'background-image': this.lightmode ? null : this.raw ? `url(${this.image.url})` : `url(${this.image.url}?thumbnail&size=512)`
			};
		}
	}
});
</script>

<style lang="stylus" scoped>
.mk-media-image
	display block
	overflow hidden
	width 100%
	height 100%
	background-position center
	background-size cover
	border-radius 4px

</style>
