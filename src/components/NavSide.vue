<template>
	<div class="nav-side" :class="{customizing: isSort}">
		<transition name="fade">
			<div v-if="isSort">
				<div class="tip"></div>
				<div class="custom-bg"></div>
			</div>
		</transition>
		<div class="nav-list">
			<div class="n-i sotrable" :class="{on: current===index}" v-for="(item, index) in data" @click="setEnable(index)" @mousedown="dragStart($event, index)">
				<div class="name">{{item.name}}</div>
			</div>
			<div class="n-i customize" @click="sort">
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
			time: 800,
			isSort: false,
			dragId: 0, //拖拽元素id (0-..)
			isDrag: false,  //当前是否在拖拽
			offsetX: 0,
			offsetY: 0,
			x: 0,
			y: 0
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
			this.bindEvent()
		},
				/*  绑定事件  */
		bindEvent() {
			// document.addEventListener('scroll', this.scroll, false) //文档对象加滚动
			document.addEventListener('mousemove', this.dragMove, false)
			document.addEventListener('mouseup', this.dragEnd, false)
			document.addEventListener('mouseleave', this.dragEnd, false)
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
		},
		sort() {
			this.isSort = !this.isSort
		},
		/*  得到鼠标位置  */
		getPos(e) {
			this.x = e.clientX - this._left - this.offsetX
			this.y = e.clientY - this._top - this.offsetY
		},
		/*  拖拽开始  */
		dragStart(e, i) {
			if (!this.isSort)
				return false
			this.isDrag = true
			console.log(e)
			this.dragId = i
			this.offsetX = e.offsetX
			this.offsetY = e.offsetY
			this.getPos(e)
			// console.log('拖拽开始')
		},
		/*  拖拽中得到鼠标位置  */
		dragMove(e) {
			if (this.isDrag) {
				console.log('获取鼠标位置')
				this.getPos(e)
			}
			e.preventDefault()
			// console.log('mousemove事件')
		},
		/*  排序拖拽结束  */
		dragEnd(e) {
			console.log('mouseleave事件')
			// if (this.isDrag) {
			// 	this.isDrag = false
			// 	if (this.exchangeId !== this.dragId) {
			// 		this.options.bindData.splice(this.exchangeId, 0, this.options.bindData.splice(this.dragId, 1)[0])
			// 	}
			// 	else {
			// 		this.setActive(this.dragId, true)
			// 	}
			// }
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
		&.customizing
			z-index 10010
			.n-i.sotrable
				cursor move
		.nav-list
			position relative
			background-color #f6f9fa
			border 1px solid #e5e9ef
			overflow hidden
			border-radius 4px
			z-index 233
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
			z-index 50
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
		.tip
			background url(http://p1.bqimg.com/567571/f0b9b188ab580a2b.png) 0 0 no-repeat
			position absolute
			left -117px
			top 0px
			width 117px
			height 333px
			z-index 10
			transition all .3s
		.custom-bg
			position absolute
			top -15px
			left -130px
			height 100%
			width 200px
			padding-bottom 35px
			box-sizing content-box
			background rgba(255,255,255,0.8)
			border-radius 4px
			z-index 5
			transition all .3s
		.fade-enter-actice, .fade-leave-active
			transition opacity .3s
		.fade-enter, .fade-leave-active
			.tip
				top 50px
				opacity 0
			.custom-bg
				top 150px
				left -70px
				height 100px
				width 100px
				opacity 0
</style>