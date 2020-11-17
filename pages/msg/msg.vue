<template>
	<view>
        <view class="chat-list">
			<view class="chat" v-for="(chat,index) in chatList" :key="index">
				<view class="row" @tap="toChat(chat)">
					<view class="left">
						<image :src="chat.face"></image>
					</view>
					<view class="right">
						<view class="top">
							<view class="username">{{chat.username}}</view>
							<view class="time">{{chat.time}}</view>
						</view>
						<view class="bottom">
							<view class="msg">{{chat.msg}}</view>
							<view class="tis" v-if="chat.tisNum>0">{{chat.tisNum}}</view>
						</view>
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
				chatList:[
					{
						uid:1,
						username:"荔波蜜柚",
						face:"/static/img/im/face/face_1.jpg",
						time:"13:08",
						msg:"亲，我们的蜜柚都是最新鲜的蜜柚喔~",
						tisNum:1
					},
					// {
					// 	uid:2,
					// 	username:"荔波罗甸玉专卖",
					// 	face:"/static/img/im/face/face_2.jpg",
					// 	time:"13:05",
					// 	msg:"我们卖的罗甸玉都是精雕正品~",
					// 	tisNum:0
					// },
					// {
					// 	uid:3,
					// 	username:"绿壳鸡蛋",
					// 	face:"/static/img/im/face/face_3.jpg",
					// 	time:"12:15",
					// 	msg:"正品绿壳鸡蛋，保证蛋味鲜美",
					// 	tisNum:0
					// },
					// {
					// 	uid:4,
					// 	username:"荔波茶叶专卖",
					// 	face:"/static/img/im/face/face_4.jpg",
					// 	time:"12:11",
					// 	msg:"感谢您购买本店的 小七孔门票+云雾茶套餐~",
					// 	tisNum:0
					// },
					// {
					// 	uid:5,
					// 	username:"荔波小七孔腾杨客栈",
					// 	face:"/static/img/im/face/face_5.jpg",
					// 	time:"12:10",
					// 	msg:"请您于明日中午来前台扫码入住~",
					// 	tisNum:0
					// },
					// {
					// 	uid:6,
					// 	username:"荔波天龙酒店",
					// 	face:"/static/img/im/face/face_6.jpg",
					// 	time:"12:10",
					// 	msg:"请查看我们最新发布的拼团信息~",
					// 	tisNum:0
					// },
					// {
					// 	uid:7,
					// 	username:"盐酸菜",
					// 	face:"/static/img/im/face/face_7.jpg",
					// 	time:"11:56",
					// 	msg:"我们的盐酸菜色鲜味美、清香脆嫩，欢迎来品尝~",
					// 	tisNum:0
					// },
					// {
					// 	uid:8,
					// 	username:"风猪",
					// 	face:"/static/img/im/face/face_8.jpg",
					// 	time:"11:56",
					// 	msg:"我们和天眼现在有拼团套餐呢，你可以去看一下~",
					// 	tisNum:0
					// },
					

				]
			}
		},
		//下拉刷新，需要自己在page.json文件中配置开启页面下拉刷新 "enablePullDownRefresh": true
		onPullDownRefresh() {
		    setTimeout(function () {
		        uni.stopPullDownRefresh();
		    }, 1000);
		},
		onLoad() {

		},
		methods: {
			toChat(chat){
				uni.navigateTo({
					url:"chat/chat?name="+chat.username
				})
			}
		}
	}
</script>

<style  lang="scss">
	page{
		background-color: rgba(0,0,0,0);
	}
	.chat-list{
		width: 94%;
		padding: 0 3%;
		.chat{
			width: 100%;
			height: 100upx;
			padding: 20upx 0;
			border-bottom: solid 1upx #eaeaea;
			.row{
				display: flex;
				justify-content: flex-start;
				.left{
					flex-shrink:0;
					width: 100upx;
					height: 100upx;
					image{
						width: 100upx;
						height: 100upx;
						border-radius: 20upx;
					}
				}
				.right{
					flex-shrink:1;
					width: 98%;
					padding-left: 2%;
					.top{
						width: 100%;
						display: flex;
						justify-content: space-between;
						align-items: flex-end;
						.usernam{
							font-size: 26upx;
						}
						.time{
							font-size: 22upx;
							color: #bebebe;
						}
					}
					.bottom{
						width: 100%;
						height: 40upx;
						display: flex;
						justify-content: space-between;
						align-items: center;
						.msg{
							font-size: 25upx;
							color: #777;
						}
						.tis{
							width: 35upx;
							height: 35upx;
							font-size: 22upx;
							display: flex;
							justify-content: center;
							align-items: center;
							background-color: #eb4d3d;
							color: #fff;
							border-radius: 100%;
						}
					}
				}
			}
		}
	}
</style>
