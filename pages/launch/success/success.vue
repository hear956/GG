<template>
	<view>
		<view class="icon">
			<image src="/static/img/success.png"></image>
		</view>
		<view class="tis">
			发布拼团成功
		</view>
		<view class="pay-amount">
			请在主页查看
		</view>
		<view class="back">
			<view class="btn" @tap="toUser">返回主页</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				amount:0
			};
		},
		onLoad(e) {
			this.amount = parseFloat(e.amount).toFixed(2);
			
		
		},
		methods: {
			toUser() {
				uni.switchTab({
					url: '/pages/tabBar/index/index'
				});
			}
		},
	}
</script>

<style lang="scss">
	view{
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
.icon{
	width: 100%;
	margin-top: 10vw;
	image{
		width: 25vw;
		height: 25vw;
	}
}
.tis{
	width: 100%;
	margin-top: 20upx;
	font-size: 48upx;
}
.pay-amount{
	width: 100%;
	margin-top: 10upx;
	font-size: 32upx;
}
.back{
	position: absolute;
	width: 100%;
	bottom: 80upx;
	.btn{
		padding: 0 50upx;
		height: 70upx;
		border: solid 2upx #595BB3;
		color: #595BB3;
		align-items: center;
		border-radius: 10upx;
		font-size: 34upx;
	}
}
</style>
