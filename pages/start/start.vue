<template>
	<view>
		<view class="content">
			<view class="row">
				<view class="nominal">
<<<<<<< HEAD
					商品信息
=======
					选择商品
>>>>>>> 9a4259a2b3a4e734a3683d63df1228f250399960
				</view>
				<view class="input">
					<!-- <input placeholder="请为此次拼团设置主题" type="text" v-model="name1" /> -->
					<picker @change="bindPickerChange" :range="array">	
							<!-- <label>国籍：</label> -->
							<label class="">{{array[index]}}</label>
						</picker>
				</view>
			</view>
			<view class="row">
				<view class="nominal">
					团购类型
				</view>
				<!-- <view class="input">
					<input placeholder="个人拼团/任务拼团" type="text" v-model="name" />
				</view> -->
				<view class="input">
					<input placeholder="请为此次拼团设置主题" type="text" v-model="name1" />
				</view>
				<!-- <radio-group name="radio" class="radio"> -->
					<!-- <label class="label">
						<radio value="radio1" checked="checked"/><text>个人拼团</text>
					</label> -->
					<!-- <label class="label">
						<radio value="radio2" /><text>任务拼团</text>
					</label> -->
				<!-- </radio-group> -->
			</view>
			<view class="row">
				<view class="nominal">
					商品数量
				</view>
				<view class="input">
					<view class="number">
						<view class="sub" @tap.stop="sub">
							<view class="icon jian"></view>
						</view>
						<view class="input" @tap.stop="discard">
							<input type="number" v-model="number" />
						</view>
						<view class="add"  @tap.stop="add">
							<view class="icon jia"></view>
						</view>
					</view>
				</view>
			</view>
			<view class="row">
				<view class="nominal">
					团购价格
				</view>
				<!-- <view class="input" @tap="chooseCity">
					{{region.label}}
				</view> -->
				<view class="input">
					<picker @change="bindPickerChange" :range="array1">	
							<label class="">￥{{array1[index]}}元</label>
						</picker>
				</view>

			</view>
		
		
		</view>
		<view class="save" @tap="save">
			<view class="btn">
				发起团购
			</view>
		</view>
		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValue" @onCancel="onCancel"
		 @onConfirm="onConfirm"></mpvue-city-picker>
	</view>
</template>


<script>
	import mpvueCityPicker from '@/components/mpvue-citypicker/mpvueCityPicker.vue'
	export default {
		components: {
			mpvueCityPicker
		},
		data() {
			return {
				editType: 'edit',
				id: '',
				name: '',
				tel: '',
				urls: [],
				detailed: '',
				name1: '',
				isDefault: false,
				cityPickerValue: [0, 0, 1],
				themeColor: '#007AFF',
				region: {
					label: "请点击选择地址",
					value: [],
					cityCode: ""
				},
				array: ["小七孔一日游","黄果树三日游","贵州七日游"],
				array1: ["100","300","600"],
				index: 0,
				number: 0
				
			};
		},
		watch: {
			urls(val) {
				console.log(val)
				this.urls = val
			}
		},
		methods: {
			//减少数量
			sub(){
				if(this.number<=1){
					return;
				}
				this.number--;
			},
			//增加数量
			add(){
				this.number++;
			},
			discard() {
				//丢弃
			},
			bindPickerChange: function(e) {		//改变的事件名
				//console.log('picker发送选择改变，携带值为', e.target.value)   用于输出改变索引值
				this.index = e.target.value			//将数组改变索引赋给定义的index变量
				// this.jg=this.array[this.index]		//将array【改变索引】的值赋给定义的jg变量
			//	console.log("籍贯为：",this.jg)		//输出获取的籍贯值，例如：中国
			},
			choose() {
				var that =this
				wx.chooseImage({
					success: function(res) {
						that.urls.push(res.tempFilePaths[0]) 
						console.log(this.urls)
						// document.getElementById("imageChoose").
					},
				});
			},
			onCancel(e) {
				console.log(e)
			},
			chooseCity() {
				this.$refs.mpvueCityPicker.show()
			},
			onConfirm(e) {
				this.region = e;
				this.cityPickerValue = e.value;
			},
			isDefaultChange(e) {
				this.isDefault = e.detail.value;
			},

			save() {
				let data = {
					"name": this.name,
					"head": this.name.substr(0, 1),
					"name1": this.name1,
					"tel": this.tel,
					"address": {
						"region": this.region,
						"detailed": this.detailed
					},
					"isDefault": this.isDefault
				}
				if (this.editType == 'edit') {
					data.id = this.id
				}
				/* if (!data.name1) {
					uni.showToast({
						title: '请输入拼团标题',
						icon: 'none'
					});
					return;
				} */
				// if(!data.name){
				// 	uni.showToast({title:'请选择拼团类型',icon:'none'});
				// 	return ;
				// }
				/* if (!data.tel) {
					uni.showToast({
						title: '请输入拼团相关信息',
						icon: 'none'
					});
					return;
				}
				if (!data.address.detailed) {
					uni.showToast({
						title: '请输入商家详细地址',
						icon: 'none'
					});
					return;
				}
				if (data.address.region.value.length == 0) {
					uni.showToast({
						title: '请选择商家所在地区',
						icon: 'none'
					});
					return;
				} */
				uni.showLoading({
					title: '正在提交'
				})
				//实际应用中请提交ajax,模板定时器模拟提交效果
				setTimeout(() => {
					uni.setStorage({
						key: 'saveAddress',
						data: data,
						success() {
							uni.hideLoading();
							uni.navigateTo({
								url: "./success/success"
							})
							// uni.navigateBack();
						}
					})
				}, 300)


			}
		},
		onLoad(e) {
			//获取传递过来的参数

			this.editType = e.type;
			if (e.type == 'edit') {
				uni.getStorage({
					key: 'address',
					success: (e) => {
						this.id = e.data.id;
						this.name = e.data.name;
						this.tel = e.data.tel;
						this.detailed = e.data.address.detailed;
						this.isDefault = e.data.isDefault;
						this.cityPickerValue = e.data.address.region.value;
						this.region = e.data.address.region;
					}
				})
			}

		},
		onBackPress() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel();
				return true;
			}
		},
		onUnload() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel()
			}
		}
	};
</script>
<style lang="scss">
	.addImg {
		display: flex;
		display: -webkit-flex;
		justify-content: space-between;
		flex-direction: row;
		flex-wrap: wrap;
		
		image{
			width: 200rpx;
			height: 200rpx;
			margin: 10rpx;
		}
	}
	.save {
		view {
			display: flex;
		}

		position: fixed;
		bottom: 0;
		width: 100%;
		height: 120upx;
		display: flex;
		justify-content: center;
		align-items: center;

		.btn {
			box-shadow: 0upx 5upx 10upx rgba(0, 0, 0, 0.4);
			width: 70%;
			height: 80upx;
			border-radius: 80upx;
			background-color: #595BB3;
			color: #fff;
			justify-content: center;
			align-items: center;

			.icon {
				height: 80upx;
				color: #fff;
				font-size: 30upx;
				justify-content: center;
				align-items: center;
			}

			font-size: 30upx;
		}
	}
.number{
				display: flex;
				justify-content: center;
				align-items: center;
				.input{
					width: 80upx;
					height: 20upx;
					margin: 0 10upx;
					background-color: #f3f3f3;
					display: flex;
					justify-content: center;
					align-items: center;
					text-align: center;
					input{
						width: 80upx;
						height: 60upx;
						display: flex;
						justify-content: center;
						align-items: center;
						text-align: center;
						font-size: 26upx;
					}
				}
				
				.sub ,.add{
					width: 60upx;
					height: 60upx;
					background-color: #f3f3f3;
					border-radius: 5upx;
					.icon{
						font-size: 30upx;
						width: 60upx;
						height: 60upx;
						display: flex;
						justify-content: center;
						align-items: center;
						
					}
				}
			}
	.content {
		display: flex;
		flex-wrap: wrap;

		view {
			display: flex;
		}

		.row {
			width: 94%;
			margin: 0 3%;
			border-top: solid 1upx #eee;

			.radio{
				margin-top: 35rpx;
				.label{
					margin-right: 10rpx;
				}
			}

			.nominal {
				width: 30%;
				height: 120upx;
				font-weight: 200;
				font-size: 30upx;
				align-items: center;
			}

			.input {
				width: 70%;
				padding: 20upx 0;
				align-items: center;
				font-size: 30upx;

				&.switch {
					justify-content: flex-end;
				}

				.textarea {
					margin: 20upx 0;
					min-height: 120upx;
				}
			}

			.del {
				width: 100%;
				height: 100upx;
				justify-content: center;
				align-items: center;
				font-size: 36upx;
				color: #595BB3;
				background-color: rgba(255, 0, 0, 0.05);
				border-bottom: solid 1upx #eee;
			}
		}
	}
</style>
