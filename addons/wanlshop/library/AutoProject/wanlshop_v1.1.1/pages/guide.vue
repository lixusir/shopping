<template>
	<view class="wanlguide-container">
		<swiper :style="{ height: $wanlshop.wanlsys().windowHeight + 'px' }" circular="true" :duration="duration"
			@change="swiperChange">
			<swiper-item class="item text-center" :style="{ backgroundColor: item.url.split('/')[2] }"
				v-for="(item, index) in common.adData.firstAdverts" :key="index">
				<view class="title" :style="{ marginTop: height + 'px' }">
					<view class="">{{ item.url.split('/')[0] }}</view>
					<text>{{ item.url.split('/')[1] }}</text>
				</view>
				<image :src="$wanlshop.oss(item.media, 280, 0, 1, 'transparent', 'png')" mode="aspectFit"></image>
			</swiper-item>
		</swiper>
		<view class="button" :style="{ top: statusBar + 'px' }">
			<button class="cu-btn round" @tap="launchFlag">{{ jumpover }}</button>
		</view>
		<view class="indicator text-df" v-if="current != common.adData.firstAdverts.length-1">
			<view class="margin-lr-xs" v-for="(item, index) in common.adData.firstAdverts" :key="index">
				<text class="wlIcon-xuanzeanniudian" v-if="current == index"></text>
				<text class="wlIcon-xuanze" v-else></text>
			</view>
		</view>
		<view class="experience animation-scale-down" v-else>
			<button class="cu-btn round lg" @tap="launchFlag">{{ experience }}</button>
		</view>
	</view>
</template>

<script>
	import {
		mapState
	} from 'vuex';
	export default {
		data() {
			return {
				height: 0,
				statusBar: 0,
				current: 0,
				duration: 500,
				jumpover: '跳过',
				experience: '立即体验'
			};
		},
		computed: {
			...mapState(['common'])
		},
		onReady() {
			let wanlsys = this.$wanlshop.wanlsys();
			// #ifdef APP-PLUS
			this.height = wanlsys.height - wanlsys.top + 20;
			this.statusBar = wanlsys.top - 10;
			// #endif
			// #ifdef MP
			this.height = wanlsys.height;
			this.statusBar = wanlsys.top;
			// #endif
			// #ifdef H5
			this.height = wanlsys.height;
			this.statusBar = 15;
			// #endif
		},
		methods: {
			swiperChange(e) {
				this.current = e.detail.current;
			},
			launchFlag() {
				// 向本地存储中设置launchFlag的值，即启动标识；
				uni.setStorage({
					key: 'wanlshop:launch',
					data: true
				});
				this.$wanlshop.to('/pages/home', 'none', 0);
			}
		}
	};
</script>

<style lang="scss">
	.cu-btn.lg {
		font-size: 30rpx;
	}

	.cu-btn:not([class*="bg-"]) {
		background-color: rgba(255, 255, 255, .5);
	}

	.wanlguide-container {
		position: relative;

		swiper-item {
			.title {
				margin-bottom: 40rpx;
				line-height: 1.4;
				color: rgba(255, 255, 255, 0.8);

				&>view {
					font-size: 62rpx;
				}

				&text {
					font-size: 32rpx;
					font-weight: 300;
				}
			}
		}

		image {
			height: 71%;
		}

		.button {
			position: absolute;
			/* #ifdef MP */
			left: 25rpx;
			/* #endif */
			/* #ifndef MP */
			right: 25rpx;

			/* #endif */
			.cu-btn {
				padding: 0 25rpx;
				font-size: 28rpx;
				height: 56rpx;
				background-color: rgba(0, 0, 0, .2);
				color: rgba(255, 255, 255, .6);
			}
		}

		.experience {
			position: absolute;
			left: 0;
			right: 0;
			bottom: 100rpx;
			display: flex;
			justify-content: center;
		}

		.indicator {
			position: absolute;
			left: 0;
			right: 0;
			bottom: 120rpx;
			display: flex;
			justify-content: center;
			color: rgba(255, 255, 255, 0.8);
		}
	}
</style>
