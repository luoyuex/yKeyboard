<template>
	<view class="y-keyboard" :class="is_open ? 'open_y_keyboard' : ''">
		<view class="vis" @click="close" v-show="is_open"></view>
		<view class="keyboard" :class="is_open ? '' : 'close_keyboard'">
			<view class="list">
				<view class="item"
					@touchstart="view_touchstart(item)"
					@touchend="view_touchend"
					@longpress="view_longpress(item)"
					@touchcancel="view_touchend"
					@click="Ykeyup(item)"
					v-for="(item, index) in random ? keys1 : keys"
					:key="index">
					<view v-if='item == "close"'>
						<slot name="close">关闭</slot>
					</view>
					<view v-else-if='item == "del"'>
						<slot name="delete">删除</slot>
					</view>
					<view v-else>
						{{item}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_open: false,
				keys: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "close", "0", "del"],
				keys1: this.shuffleKeys()
			}
		},
		props: {
			random: {
				type: Boolean,
				default: () => {
					return false
				}
			},
			openRandom: {
				type: Boolean,
				default: () => {
					return false
				}
			}
		},
		methods: {
			shuffleKeys() {
				let numberKeys = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"];
				// Fisher-Yates Shuffle 算法
				for (let i = numberKeys.length - 1; i > 0; i--) {
					const j = Math.floor(Math.random() * (i + 1));
					[numberKeys[i], numberKeys[j]] = [numberKeys[j], numberKeys[i]];
				}
				// 添加固定的 "close" 和 "del"
				return [...numberKeys.slice(0, 9), "close", numberKeys[9], "del"];
			},
			view_longpress(item){
				if(item !== "del") return;
				this.$emit('longpress');
			},
			view_touchstart(item){
				this.is_hover = item;
			},
			view_touchend(){
				this.is_hover = "";
			},
			show() {
				if(this.openRandom) {
					this.keys1 = this.shuffleKeys();
				}
				this.is_open = true;
				console.log(this.is_open);
			},
			close() {
				this.is_open = false;
			},
			Ykeyup(item) {
				switch (item) {
					case "close":
						// 收起键盘
						this.close();
						break;
					case "1":
					case "2":
					case "3":
					case "4":
					case "5":
					case "6":
					case "7":
					case "8":
					case "9":
					case "0":
						this.$emit('input', item)
						break;
					case "del":
						this.$emit('delete');
						break;
						
				}
			}
		}
	}
</script>

<style scoped>
	.y-keyboard {
		position: absolute;
		width: 100%;
		bottom: 0;
		left: 0;
	}
	.y-keyboard .keyboard .list {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		height: 100%;
		user-select: none; /* 标准属性 */
		-webkit-user-select: none; /* 兼容 WebKit 浏览器 */
		-moz-user-select: none; /* 兼容 Firefox */
		-ms-user-select: none; /* 兼容老版 IE */
	}
	.y-keyboard .keyboard .list .item {
		width: 32%;
		height: calc(25% - 10rpx);
		background: #FFF;
		border-radius: 16rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.y-keyboard .keyboard .list .item:active {
	  background-color: #bdc5d0; /* 按下时背景颜色 */
	  /* color: white; */
	}
	.open_y_keyboard {
		height: 100%;
	}
	.y-keyboard .vis {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;
		background-color: transparent;
	}
	
	.y-keyboard .keyboard {
	    width: 100%;
	    height: 510rpx;
	    background-color: rgba(250, 250, 250, 1);
	    border-radius: 20rpx 20rpx 0 0;
	    position: absolute;
	    bottom: 0px;
	    padding: 20rpx;
	    box-sizing: border-box;
	    transition: bottom 0.3s ease-in-out; /* 添加过渡动画 */
	}
	
	.y-keyboard .close_keyboard {
	    bottom: -510rpx; /* 注意这里的单位修正 */
	}

</style>