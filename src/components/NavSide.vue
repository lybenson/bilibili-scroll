<template>
	<div class="nav-side">
		<transition>
			<div>
				<div class="tip"></div>
				<div class="custom-bg"></div>
			</div>
		</transition>
		<div class="nav-list">
			<div class="n-i sotrable" :class="{on: current===index}" v-for="(item, index) in data" @click="setEnable(index)">
				<div class="name">{{item.name}}</div>
			</div>
			<div class="n-i customize">
				<i class="n-icon-sort"></i>
				<p>排序</p>
			</div>
		</div>
		<div class="n-i gotop">
			<div class="s-line"></div>
			<div class="btn_gotop"></div>
		</div>
	</div>
</template>

<script>
import scrollMixin from './smooth-scroll.js'
export default {
	mixins: [scrollMixin],
	data() {
		return {
			current: 0,
			data: [],
			time: 800
		}
	},
	props: {
		options: {
			type: Object
		}
	},
	mounted() {
		this.init()
	},
	methods: {
		init() {
			console.log('初始化数据')
			this.initData()
		},
		initData() {
			console.log('initData')
			console.log(this.options.items)
			this.data = Array.from(this.options.items, (d) => {
				console.log('单个元素为'+JSON.stringify(d))
				let element = document.getElementById(`b_${d.desc}`)
				if (!element) {

					return
				}
				let offsetTop = this.getOffsetTop(element)
				console.log(offsetTop)
				return {
					name: d.name,
					element: element,
					offsetTop: offsetTop,
					height: element.offsetHeight
				}
			}) 
		},
		setEnable(index) {
			if (index === this.current) {
				return false
			}
			this.current = index
			let target = this.data[index].element
			this.scrollToElem(target, this.time, this.offset || 0).then(() => {
				// this.queueNumber--
				// if (this.queueNumber === 0) {
				// 	this.isClickScroll = false
				// }
			})
		},
		//获取元素距离顶部的距离
		getOffsetTop(element) {
			var top, clientTop, clientLeft, scrollTop, scrollLeft,
			doc = document.documentElement,
			body = document.body;
			if (typeof element.getBoundingClientRect !== "undefined") {
				top = element.getBoundingClientRect().top;
			} else {
				top = 0;
			}
			clientTop = doc.clientTop || body.clientTop || 0;
			scrollTop = window.pageYOffset || doc.scrollTop;
			return (top + scrollTop - clientTop);
		}
	}
}
</script>

<style lang="stylus" scoped>
	.nav-side
		position fixed
		width 48px
		text-align center
		top 0px
		left auto
		right 0px
		.nav-list
			position relative
			background-color #f6f9fa
			border 1px solid #e5e9ef
			overflow hidden
			border-radius 4px
			.n-i
				cursor pointer
				&:first-child
					border-top 0
				&.on
					.name
						background-color #00a1d6
						color #fff
				&.customize
					padding 8px 0
					border-top 1px solid #e5e9ef
					.n-icon-sort
						display block
						margin 0 auto 4px
						background url(../assets/icons.png) -663px -151px no-repeat
						height 18px
						width 18px
				.name
					height 32px
					line-height 32px
					transition .1s linear
					transition-property background-color, color
					&:hover
						background-color #00a1d6
						color #fff
		.gotop
			cursor pointer
			border 0
			position relative
			.s-line
				border-left 1px solid #ddd
				border-right 1px solid #ddd
				height 9px
				width 30px
				margin 0 auto
			.btn_gotop
				height 48px
				background #f6f9fa url(../assets/icons.png) -648px -72px no-repeat
				border 1px solid #e5e9ef
				overflow hidden
				border-radius 4px
</style>