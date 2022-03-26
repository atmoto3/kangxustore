<template>
	<div>
		<!-- 导航栏 -->
		<div class='navBarClass'>
			<van-nav-bar >
				<template #left>
					<van-icon name="wap-home-o" color='#423939' size="78" @click='gotoHome' />
				</template>

				<template #right>
					<van-icon name="shopping-cart-o" color='#423939' size="78"  />
				</template>
			</van-nav-bar>
		</div>

		

		<!-- 登录按钮 -->
		<div class='btnWrap'>
			<van-button 
				:icon="login_icon" 
				color="#DD1A21" 
				type="primary" 
				plain 
				style="height:1.3rem;margin-bottom: .42667rem;" 
				block
				@click='loginBtnEvent'
				>
				邮箱帐号登录
			</van-button>

			<van-button 
				:icon="login_icon" 
				color="#fff" 
				type="primary" 
				plain 
				style="height:1.3rem;background-color: #dd1a21" 
				block
				@click='registerBtnEvent'
				>
				注册用户名
			</van-button>

			<span class='other' @click='otherLoginFn'>其它登录方式 > </span>
		</div>

		<!-- 用户名/密码，登录 -->
		<div class='loginWrap' v-show='loginWrap_show'>
			<input v-model='user_val' type='text' placeholder='邮箱账号'/>
			<input v-model='pass_val' type='password' placeholder='密码'/>
			<input @click='userLoginFn' type='button' style='border:0;' value='登录'/>
		</div>

		<!--用户名/密码，注册  -->
		<div class='loginWrap' v-show='register_show'>
			<input v-model='re_user_val' type='text' placeholder='注册邮箱账号'/>
			<input v-model='re_pass_val' type='password' placeholder='注册密码'/>
			<input @click='getRegisterValFn' type='button' style='border:0;' value='注册'/>
		</div>


		
		

	</div>
</template>

<script>
import login_icon from '../assets/login_icon.png'
import { Dialog } from 'vant';

export default{
	name:'login',
	data(){
		return {
			msg:'登录页',
			login_icon : login_icon,
			loginWrap_show: false,
			register_show: false,

			// 登录用户名、密码
			user_val:'',
			pass_val:'',
			
			//注册用户名、密码
			re_user_val: '', 
			re_pass_val: '',

			// 保存多个用户的注册信息
			reArrObj:[],

			// 保存获取到的所有的用户信息
			saveUserInfo:null



		}
	},
	mounted(){
		// 取出来的是字符，所以要转格式
		let _getRegObj = localStorage.getItem('reArrObj');
		console.log(JSON.parse(_getRegObj));

		this.saveUserInfo = JSON.parse(_getRegObj);
	},
	methods:{
		gotoHome(){
			this.$router.push('/');
		},

		// 显示登录容器
		loginBtnEvent(){
			this.loginWrap_show= true;
			this.register_show= false;
		},
		// 显示注册容器
		registerBtnEvent(){
			this.loginWrap_show= false;
			this.register_show= true;
		},
		// 其它登录方式
		otherLoginFn(){
			this.loginWrap_show= false;
			this.register_show= false;
		},
		// 注册，用户名、密码
		getRegisterValFn(){
			console.log(this.re_user_val,'-----',this.re_pass_val);

			// 注册用户名
			let _reObj = {
				u:this.re_user_val,
				p:this.re_pass_val
			}

			this.reArrObj.push( _reObj );

			// 保存注册信息
			localStorage.setItem('reArrObj',JSON.stringify(this.reArrObj));

			// 重置注册输入框
			this.re_user_val= '';
			this.re_pass_val= '';
		},
		// 用户登录
		userLoginFn(){
			// console.log(this.user_val,'-----',this.pass_val);

			// for循环判断是否已经登录，这只是为了逻辑通顺，简单实现
			for(var i=0; i<this.saveUserInfo.length; i++){
				// console.log( this.saveUserInfo[i] );
				if (this.saveUserInfo[i].u === this.user_val && 
				this.saveUserInfo[i].p === this.pass_val){
					// console.log('已经成功登录');
					Dialog.alert({
						title: '登录成功',
						message: '欢迎回来:'+ this.user_val + '您好',
						}).then(() => {
							this.$router.push('/');
						// on close
						});		
									
					break;
				}
				Dialog.alert({
						title: '登录失败',
						message: '请注册用户',
						}).then(() => {
							this.user_val= '';
							this.pass_val= '';
						// on close
						});			
				
			}

		}
	}
}
</script>

<style scoped>
.logoWrap {
    text-align: center;
    padding-top: 2.13333rem;
    padding-bottom: 3.09333rem;
}
.logoWrap img {
    width: 3.57333rem;
    height: 1.2rem;
}

.btnWrap {
    padding: 0 .53333rem;
    margin-bottom: 1.73333rem;
}

.btnWrap span.other{
	text-align: center;
	display: block;
	font-size: .37333rem;
	margin-top: .42667rem;


}

.loginWrap{
	text-align:center;
}

.loginWrap input{
	display: block;
	margin: .3rem auto;
	width: 4rem;
	height: .7rem;
	font-size: .37333rem;
	border: 0;
	border-bottom:1px solid #666;
}
</style>