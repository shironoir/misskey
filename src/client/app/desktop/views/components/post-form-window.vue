<template>
<mk-window ref="window" is-modal @closed="$destroy">
	<span slot="header">
		<span :class="$style.icon" v-if="geo">%fa:map-marker-alt%</span>
		<span v-if="!reply">%i18n:@note%</span>
		<span v-if="reply">%i18n:@reply%</span>
		<span :class="$style.count" v-if="media.length != 0">{{ '%i18n:@attaches%'.replace('{}', media.length) }}</span>
		<span :class="$style.count" v-if="uploadings.length != 0">{{ '%i18n:@uploading-media%'.replace('{}', uploadings.length) }}<mk-ellipsis/></span>
	</span>

	<mk-note-preview v-if="reply" :class="$style.notePreview" :note="reply"/>
	<mk-post-form ref="form"
		:reply="reply"
		@posted="onPosted"
		@change-uploadings="onChangeUploadings"
		@change-attached-media="onChangeMedia"
		@geo-attached="onGeoAttached"
		@geo-dettached="onGeoDettached"/>
</mk-window>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
	props: ['reply'],
	data() {
		return {
			uploadings: [],
			media: [],
			geo: null
		};
	},
	mounted() {
		this.$nextTick(() => {
			(this.$refs.form as any).focus();
		});
	},
	methods: {
		onChangeUploadings(files) {
			this.uploadings = files;
		},
		onChangeMedia(media) {
			this.media = media;
		},
		onGeoAttached(geo) {
			this.geo = geo;
		},
		onGeoDettached() {
			this.geo = null;
		},
		onPosted() {
			(this.$refs.window as any).close();
		}
	}
});
</script>

<style lang="stylus" module>
.icon
	margin-right 8px

.count
	margin-left 8px
	opacity 0.8

	&:before
		content '('

	&:after
		content ')'

.notePreview
	margin 16px 22px

</style>
