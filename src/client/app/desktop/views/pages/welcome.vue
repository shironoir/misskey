<template>
<div class="mk-welcome">
	<button @click="dark">
		<template v-if="_darkmode_">%fa:moon%</template>
		<template v-else>%fa:R moon%</template>
	</button>
	<main>
		<img :src="_darkmode_ ? 'assets/title-dark.svg' : 'assets/title.svg'" alt="Misskey">
		<p><button class="signup" @click="signup">%i18n:@signup-button%</button><button class="signin" @click="signin">%i18n:@signin-button%</button></p>

		<div class="tl">
			<header>%fa:comments R% %i18n:@timeline%<div><span></span><span></span><span></span></div></header>
			<mk-welcome-timeline/>
		</div>
	</main>
	<mk-forkit/>
	<footer>
		<div>
			<mk-nav :class="$style.nav"/>
			<p class="c">{{ copyright }}</p>
		</div>
	</footer>
	<modal name="signup" width="500px" height="auto" scrollable>
		<header :class="$style.signupFormHeader">%i18n:@signup%</header>
		<mk-signup :class="$style.signupForm"/>
	</modal>
	<modal name="signin" width="500px" height="auto" scrollable>
		<header :class="$style.signinFormHeader">%i18n:@signin%</header>
		<mk-signin :class="$style.signinForm"/>
	</modal>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { docsUrl, copyright, lang } from '../../../config';

export default Vue.extend({
	data() {
		return {
			aboutUrl: `${docsUrl}/${lang}/about`,
			copyright
		};
	},
	methods: {
		signup() {
			this.$modal.show('signup');
		},
		signin() {
			this.$modal.show('signin');
		},
		dark() {
			(this as any)._updateDarkmode_(!(this as any)._darkmode_);
		}
	}
});
</script>

<style>
#wait {
	right: auto;
	left: 15px;
}
</style>

<style lang="stylus" scoped>
@import '~const.styl'

root(isDark)
	display flex
	flex-direction column
	flex 1

	> button
		position absolute
		z-index 1
		top 0
		left 0
		padding 16px
		font-size 18px
		color isDark ? #fff : #555

	> main
		flex 1
		padding 64px 0 0 0
		text-align center
		color isDark ? #9aa4b3 : #555

		> img
			width 350px

		> p
			margin 8px 0
			line-height 2em

			button
				padding 8px 16px
				font-size inherit

			.signup
				color $theme-color
				border solid 2px $theme-color
				border-radius 4px

				&:focus
					box-shadow 0 0 0 3px rgba($theme-color, 0.2)

				&:hover
					color $theme-color-foreground
					background $theme-color

				&:active
					color $theme-color-foreground
					background darken($theme-color, 10%)
					border-color darken($theme-color, 10%)

			.signin
				&:hover
					color isDark ? #fff : #000

		> .tl
			margin 32px auto 0 auto
			width 410px
			text-align left
			background isDark ? #313543 : #fff
			border-radius 8px
			box-shadow 0 8px 32px rgba(#000, 0.15)
			overflow hidden

			> header
				z-index 1
				padding 12px 16px
				color isDark ? #e3e5e8 : #888d94
				box-shadow 0 1px 0px rgba(#000, 0.1)

				> div
					position absolute
					top 0
					right 0
					padding inherit

					> span
						display inline-block
						height 11px
						width 11px
						margin-left 6px
						border-radius 100%
						vertical-align middle

						&:nth-child(1)
							background #5BCC8B

						&:nth-child(2)
							background #E6BB46

						&:nth-child(3)
							background #DF7065

			> .mk-welcome-timeline
				max-height 350px
				overflow auto

	> footer
		font-size 12px
		color isDark ? #949ea5 : #737c82

		> div
			margin 0 auto
			padding 64px
			text-align center

			> .c
				margin 16px 0 0 0
				font-size 10px
				opacity 0.7

.mk-welcome[data-darkmode]
	root(true)

.mk-welcome:not([data-darkmode])
	root(false)

</style>

<style lang="stylus" module>
.signupForm
	padding 24px 48px 48px 48px

.signupFormHeader
	padding 48px 0 12px 0
	margin: 0 48px
	font-size 1.5em
	color #777
	border-bottom solid 1px #eee

.signinForm
	padding 24px 48px 48px 48px

.signinFormHeader
	padding 48px 0 12px 0
	margin: 0 48px
	font-size 1.5em
	color #777
	border-bottom solid 1px #eee

.nav
	a
		color #666
</style>
