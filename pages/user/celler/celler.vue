<template>
	<view>
		<view class="content">
			<view class="row">
				<view class="nominal">
					商家名称
				</view>
				<view class="input">
					<input placeholder="请输入商家名称" type="text" v-model="sellname" />
				</view>
			</view>
			<view class="row">
				<view class="nominal">
					商家法定人
				</view>
				<view class="input">
					<input placeholder="请输入法定人相关信息" type="text" v-model="name" />
				</view>
				
			</view>
			<view class="row">
				<view class="nominal">
					商家联系方式
				</view>
				<view class="input">
					<input placeholder="请输入商家联系方式" type="number" v-model="number1" />
				</view>
				
			</view>
			<view class="row">
				<view class="nominal">
					法定人身份证
				</view>
				<view class="input">
					<input placeholder="请输入身份证号码" type="number" v-model="number" />
				</view>
				
			</view>
			<view class="row">
				<view class="nominal">
					所在地区
				</view>
				<view class="input" @tap="chooseCity">
					{{region.label}}
				</view>
				
			</view>
			<view class="row">
				<view class="nominal">
					商家地址
				</view>
				<view class="input">
					<textarea v-model="detailed" auto-height="true" placeholder="输入店铺详细地址"></textarea>
				</view>
			</view>
			<view class="row">
				<view class="nominal">
					添加商铺图片
				</view>
				<view class="input switch">
					<switch color="#f06c7a" :checked="isDefault" @change=isDefaultChange />
				</view>
			</view>
			<view class="row" v-if="editType=='edit'" @tap="del">
				
			</view>
		</view>
		<view class="save" @tap="save">
			<view class="btn">
				发布拼团
			</view>
	
		</view>
		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValue" @onCancel="onCancel" @onConfirm="onConfirm"></mpvue-city-picker>
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
				editType:'edit',
				id:'',
				name:'',
				tel:'',
				detailed:'',
				isDefault:false,
				cityPickerValue: [0, 0, 1],
				themeColor: '#007AFF',
				region:{label:"请点击选择地址",value:[],cityCode:""}
			};
		},
		
		methods: {
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
			isDefaultChange(e){
				this.isDefault = e.detail.value;
			},
			del(){
				uni.showModal({
					
					success: (res)=>{
						if (res.confirm) {
							uni.setStorage({
								key:'delAddress',
								data:{id:this.id},
								success() {
									uni.navigateBack();
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
				
			},
			save(){
				let data={"sellname":this.sellname,"name":this.name,"head":this.name.substr(0,1),"number1":this.number1,"number":this.number,"address":{"region":this.region,"detailed":this.detailed},"isDefault":this.isDefault}
				if(this.editType=='edit'){
					data.id = this.id
				}
				if(!data.sellname){
					uni.showToast({title:'请输入商家名称',icon:'none'});
					return ;
				}
				if(!data.name){
					uni.showToast({title:'请输入法定人姓名',icon:'none'});
					return ;
				}
				if(!data.number1){
					uni.showToast({title:'请输入商家联系方式',icon:'none'});
					return ;
				}
				if(!data.number){
					uni.showToast({title:'请输入法定人身份证号码',icon:'none'});
					return ;
				}
				if(!data.address.detailed){
					uni.showToast({title:'请输入店铺详细地址',icon:'none'});
					return ;
				}
				
				uni.showLoading({
					title:'正在提交'
				})
				//实际应用中请提交ajax,模板定时器模拟提交效果
				setTimeout(()=>{
					uni.setStorage({
						key:'saveAddress',
						data:data,
						success() {
							uni.hideLoading();
							uni.navigateTo({
								url:'success/success'
							})
							// uni.navigateBack();
						}
					})
				},300)
				
				
			},
			
		},
		onLoad(e) {
			//获取传递过来的参数
			
			this.editType = e.type;
			if(e.type=='edit'){
				uni.getStorage({
					key:'address',
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

.save{
		view{
			display: flex;
		}
		position: fixed;
		bottom: 0;
		width: 100%;
		height: 120upx;
		display: flex;
		justify-content: center;
		align-items: center;
		.btn{
			box-shadow: 0upx 5upx 10upx rgba(0,0,0,0.4);
			width: 70%;
			height: 80upx;
			border-radius: 80upx;
			background-color: #595BB3;
			color: #fff;
			justify-content: center;
			align-items: center;
			.icon{
				height: 80upx;
				color: #fff;
				font-size: 30upx;
				justify-content: center;
				align-items: center;
			}
			font-size: 30upx;
		}
	}
	.content{
		display: flex;
		flex-wrap: wrap;
		view{
			display: flex;
		}
		.row{
			width: 94%;
			
			margin: 0 3%;
			border-top: solid 1upx #eee;
			.nominal{
				width: 30%;
				height: 120upx;
				font-weight: 200;
				font-size: 30upx;
				align-items: center;
			}
			.input{
				width: 70%;
				padding: 20upx 0;
				align-items: center;
				font-size: 30upx;
				&.switch{
					justify-content: flex-end;
				}
				.textarea{
					margin: 20upx 0;
					min-height: 120upx;
				}
			}
			.del{
				width: 100%;
				height: 100upx;
				justify-content: center;
				align-items: center;
				font-size: 36upx;
				color: #595BB3;
				background-color: rgba(255,0,0,0.05);
				border-bottom: solid 1upx #eee;
			}
		}
	}
	
</style>
