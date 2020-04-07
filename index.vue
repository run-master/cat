<template>
	<view>
		<shouye v-if="PageCur=='shouye'"></shouye>
		<wuliudingdan v-if="PageCur == 'wuliudingdan'"></wuliudingdan>
		<fahuodingdan v-if="PageCur == 'fahuodingdan'"></fahuodingdan>
		<sjwode v-if="PageCur == 'sjwode'"></sjwode>
		<view class="cu-bar tabbar bg-white shadow foot">
			<!-- 首页 -->
			<view class="action text-yellow" @tap="NavChange" data-cur="shouye">
				<view class='cuIcon-cu-image'>
					<text class="lg text-yellow cuIcon-home" v-if="PageCur=='shouye'"></text>
					<text class="lg text-grey cuIcon-home" v-else></text>
				</view>
				<view :class="PageCur=='shouye'?'text-yellow':'text-gray'">首页</view>
			</view>
			<!-- 物流订单 -->
			<view class="action text-yellow" @tap="NavChange" data-cur="wuliudingdan">
				<view class='cuIcon-cu-image'>
					<text class="lg text-yellow cuIcon-sort" v-if="PageCur=='wuliudingdan'"></text>
					<text class="lg text-grey cuIcon-sort" v-else></text>
				</view>
				<view :class="PageCur=='wuliudingdan'?'text-yellow':'text-gray'">物流订单</view>
			</view>
			<!-- 发布车源 -->
			<!-- <view class="action text-gray add-action" @tap="NavChange" data-cur="fabucheyuan">
				<button class="cuIcon-add bg-yellow"></button>
				发布车源
			</view> -->
			<!-- 订单 -->
			<view class="action text-yellow" @tap="NavChange" data-cur="fahuodingdan">
				<view class='cuIcon-cu-image'>
					<text class="lg text-yellow cuIcon-list" v-if="PageCur=='fahuodingdan'"></text>
					<text class="lg text-grey cuIcon-list" v-else></text>
				</view>
				<view :class="PageCur=='fahuodingdan'?'text-yellow':'text-gray'">发货订单</view>
			</view>
			<!-- 个人中心 -->
			<view class="action text-yellow" @tap="NavChange" data-cur="sjwode">
				<view class='cuIcon-cu-image'>
					<text class="lg text-yellow cuIcon-profile" v-if="PageCur=='sjwode'"></text>
					<text class="lg text-grey cuIcon-profile" v-else></text>
				</view>
				<view :class="PageCur=='sjwode'?'text-yellow':'text-gray'">个人中心</view>
			</view>
		</view>
		<view class="cu-tabbar-height"></view>
		<!--显示modal-->
		<tui-modal :show="modalStatus" @click="handleModalClick" @cancel="hideModal" :content="modalContext" color="#333"
		 :size="32" shape="circle" :button="modalButton"></tui-modal>
		
	</view>
</template>

<script>
	import shouye from "@/pages/driver/shouye/home_page.vue";
	import wuliudingdan from "@/pages/wuliudingdan/wuliudingdan.vue";
	import fahuodingdan from "@/pages/fahuodingdan/fahuodingdan.vue"
	import sjwode from "@/pages/driver/wode/wode"
	
	import utilCommnon from '@/common/js/util_common.js'
	
	import{
		mapState,
		mapMutations,
		mapActions
	}from 'vuex'
	
	export default {
		computed:mapState(['isLogin','loginUserInfo']),
		components:{
			shouye,
			wuliudingdan,
			fahuodingdan,
			sjwode
		},
		data() {
			return {
				PageCur: 'shouye',
				//是否显示modal弹窗
				modalStatus: false,
				//弹窗modal显示的内容
				modalContext: '',
				//弹窗modal的按钮颜色
				modalButton: [{
					text: "取消",
					type: 'gray'
				}, {
					text: "确定"
				}],
				//进入页面的url
				intoPageUrl: ''
			}
		},
		onLoad() {
			console.log("enter guest model")
			//判断是否已经登录
			this.isUserLogin()
		},
		mounted() {
			console.log("mouted enter guest model")
			//判断是否已经登录
			this.isUserLogin()
		},
		methods: {
			//底部导航栏切换
			NavChange: function(e) {
				var _self=this
				console.log(e)
				//this.PageCur = e.currentTarget.dataset.cur
				//console.log("this.PageCur=="+this.PageCur)
				if( e.currentTarget.dataset.cur!="shouye"){
					// var intoUrl='/pages/login/login'
					// var str='请先登录'
					// utilCommnon.showIntoModalInfoNoClosePage(str,intoUrl)
					_self.modalContext='请先登录'
					_self.intoPageUrl='/pages/login/login'
					_self.showModal()
				}else{
					this.PageCur = e.currentTarget.dataset.cur
				}	
				
			},
			/**
			 * 判断用户是否已经登录
			 */
			isUserLogin:function(){
				if(this.isLogin){
					console.log(this.loginUserInfo)
					console.log(this.loginUserInfo.user_sfbz)
					if(this.loginUserInfo.user_sfbz == '1'){
						uni.reLaunch({
							url:"/pages/driver/shouye/index"
						})
					}else if(this.loginUserInfo.user_sfbz == '2'){
						uni.reLaunch({
							url:"/pages/shipper/shouye/index"
						})
					}else if(this.loginUserInfo.user_sfbz=='3'){
						uni.reLaunch({
							url:"/pages/enterprise/shouye/index"
						})
					}
				}
			},
			
			/**
			 * 显示弹窗
			 */
			showModal() {
				this.modalStatus = true
			},
			/**
			 * @param {Object} e选择按钮
			 */
			handleModalClick(e) {
				var _self = this
				let index = e.index;
				console.log("你点击的按钮index：" + index)
				//进入注册页面
				if (index == 1) {
					uni.navigateTo({
						url: _self.intoPageUrl
					})
				}
				this.hideModal()
			},
			/**
			 * 隐藏modal弹窗
			 */
			hideModal() {
				this.modalStatus = false
			},
		}
	}
</script>

<style>

</style>
