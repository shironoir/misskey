<template>
<div class="sub" :class="{ smart: $store.state.device.postStyle == 'smart' }">
	<mk-avatar class="avatar" :user="note.user" v-if="$store.state.device.postStyle != 'smart'"/>
	<div class="main">
		<header>
			<mk-avatar class="avatar" :user="note.user" v-if="$store.state.device.postStyle == 'smart'"/>
			<router-link class="name" :to="note.user | userPage">{{ note.user | userName }}</router-link>
			<span class="is-admin" v-if="note.user.isAdmin">admin</span>
			<span class="is-bot" v-if="note.user.isBot">bot</span>
			<span class="is-cat" v-if="note.user.isCat">cat</span>
			<span class="username"><mk-acct :user="note.user"/></span>
			<div class="info">
				<span class="mobile" v-if="note.viaMobile">%fa:mobile-alt%</span>
				<router-link class="created-at" :to="note | notePage">
					<mk-time :time="note.createdAt"/>
				</router-link>
				<span class="visibility" v-if="note.visibility != 'public'">
					<template v-if="note.visibility == 'home'">%fa:home%</template>
					<template v-if="note.visibility == 'followers'">%fa:unlock%</template>
					<template v-if="note.visibility == 'specified'">%fa:envelope%</template>
					<template v-if="note.visibility == 'private'">%fa:lock%</template>
				</span>
			</div>
		</header>
		<div class="body">
			<mk-sub-note-content class="text" :note="note"/>
		</div>
	</div>
</div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
	props: ['note']
});
</script>

<style lang="stylus" scoped>
root(isDark)
	padding 16px
	font-size 0.9em
	background isDark ? #21242d : #fcfcfc

	@media (min-width 600px)
		padding 24px 32px

	&.smart
		> .main
			width 100%

			> header
				align-items center

	&:after
		content ""
		display block
		clear both

	> .avatar
		display block
		float left
		margin 0 10px 0 0
		width 44px
		height 44px
		border-radius 8px

		@media (min-width 500px)
			margin-right 16px
			width 52px
			height 52px

	> .main
		float left
		width calc(100% - 54px)

		@media (min-width 500px)
			width calc(100% - 68px)

		> header
			display flex
			align-items baseline
			margin-bottom 2px
			white-space nowrap

			> .avatar
				flex-shrink 0
				margin-right 8px
				width 18px
				height 18px
				border-radius 100%

			> .name
				display block
				margin 0 0.5em 0 0
				padding 0
				overflow hidden
				color isDark ? #fff : #607073
				font-size 1em
				font-weight 700
				text-align left
				text-decoration none
				text-overflow ellipsis

				&:hover
					text-decoration underline

			> .is-admin
			> .is-bot
			> .is-cat
				margin 0 0.5em 0 0
				padding 1px 5px
				font-size 10px
				color isDark ? #758188 : #aaa
				border solid 1px isDark ? #57616f : #ddd
				border-radius 3px

				&.is-admin
					border-color isDark ? #d42c41 : #f56a7b
					color isDark ? #d42c41 : #f56a7b

			> .username
				text-align left
				margin 0
				color isDark ? #606984 : #d1d8da

			> .info
				margin-left auto
				font-size 0.9em

				> *
					color isDark ? #606984 : #b2b8bb

				> .mobile
					margin-right 6px

				> .visibility
					margin-left 6px

		> .body
			max-height 128px
			overflow hidden

			> .text
				cursor default
				margin 0
				padding 0
				font-size 1.1em
				color isDark ? #959ba7 : #717171

				pre
					max-height 120px
					font-size 80%

.sub[data-darkmode]
	root(true)

.sub:not([data-darkmode])
	root(false)

</style>
