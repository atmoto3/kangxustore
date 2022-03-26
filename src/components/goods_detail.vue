<template>
	<div style="font-size:1rem;">
		<!-- 导航栏 -->
		<div class='navBarClass'>
			<van-nav-bar >
				<template #left>
					<van-icon name="arrow-left" size="78" color='#423939' @click='gotoHome' />
				</template>

				<template #right>
					<van-icon name="shopping-cart-o" color='#423939' size="78"  />
				</template>
			</van-nav-bar>
		</div>

		<!-- 商品信息 -->
		<div class='goods_detail'>
			<img :src="goods_img" />
			<p>{{goodsName}}</p> 
			<p>{{goods_price}}</p> 
		</div>

		<!-- sku 商品规格 -->
		<div class='skuClass'>
			<van-sku v-model="show"
				:sku="sku"
				:goods="goods"
				goods-id="goodsIdxx"
				@buy-clicked="onBuyClicked"
				>
				<template slot="sku-actions-top">
					<van-uploader 
						v-model="fileList"
						:after-read="afterRead"></van-uploader>
				</template>
			</van-sku>
		</div>


		<!-- 底部按钮 -->
		<div class='goodsActionClass'>
			<van-goods-action>
				<van-goods-action-icon icon="chat-o" text="客服" @click="onClickIcon" />
				
				<van-goods-action-button color="#423939"
					type="danger" text="加入购物车"
					@click="onClickButton"
					/>
				
				<van-goods-action-button color='#ff7d00'
					type="warning" text="立即购买"
					@click="onClickButton"
					/>
				
			</van-goods-action>
		</div>
	</div>
</template>

<script>
export default{
	name:'goods_detail',
	data(){
		return{
			msg:'商品购买页',
			goodsName:'',
			goods_img:'',
			goods_price:0,
			show: false,
			// 图片预览
			fileList:[],
			// 保存上传的图片信息
			uploader_img_arr:[],
			goods: {
			  // 默认商品 sku 缩略图
			  picture: ''
			},
			sku: {
			    // 所有sku规格类目与其值的从属关系，比如商品有颜色和尺码两大类规格，颜色下面又有红色和蓝色两个规格值。
			    // 可以理解为一个商品可以有多个规格类目，一个规格类目下可以有多个规格值。
			    tree: [
					// 手机颜色
					{
			        k: '颜色',
			        // skuKeyName：规格类目名称
			        v: [{
			            id: '11',
			            // skuValueId：规格值 id
			            name: '大师纸',
			            // skuValueName：规格值名称
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 规格类目图片，只有第一个规格类目可以定义图片
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 用于预览显示的规格类目图片
			        },
			        {
			            id: '22',
			            name: '大师森',
			            imgUrl: 'https://r1.realme.net/general/20220104/1641301567983.png',
			            previewImgUrl: 'https://r1.realme.net/general/20220104/1641301567983.png',
			        },
			        {
			            id: '33',
			            name: '钛蓝',
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834573892.png',
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834573892.png',
			        },
			        {
			            id: '44',
			            name: '锻黑',
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834735878.png',
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834735878.png',
			        }],
			        k_s: 's1' // skuKeyStr：sku 组合列表（下方 list）中当前类目对应的 key 值，value 值会是从属于当前类目的一个规格值 id
			    },
				
				// 手机配置
				{
			    	k:'配置',
			    	v:[{
			            id: '111',
			            // skuValueId：规格值 id
			            name: '8GB+128GB',
			            // skuValueName：规格值名称
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 规格类目图片，只有第一个规格类目可以定义图片
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 用于预览显示的规格类目图片
			        },{
			            id: '222',
			            // skuValueId：规格值 id
			            name: '8GB+256GB',
			            // skuValueName：规格值名称
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 规格类目图片，只有第一个规格类目可以定义图片
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 用于预览显示的规格类目图片
			        },{
			            id: '333',
			            // skuValueId：规格值 id
			            name: '12GB+256GB',
			            // skuValueName：规格值名称
			            imgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 规格类目图片，只有第一个规格类目可以定义图片
			            previewImgUrl: 'https://r1.realme.net/general/20211230/1640834892904.png',
			            // 用于预览显示的规格类目图片
			        }],				
			        k_s: 's2'
			    }],


			    // 所有 sku 的组合列表，比如红色、M 码为一个 sku 组合，红色、S 码为另一个组合
			    list: [{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 259900,
			        // 价格（单位分）
			        s1: '11',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '111',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 333 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 279900,
			        // 价格（单位分）
			        s1: '11',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '222',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 309900,
			        // 价格（单位分）
			        s1: '11',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '333',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    },
				
				{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 259900,
			        // 价格（单位分）
			        s1: '22',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '111',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 555 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 289900,
			        // 价格（单位分）
			        s1: '22',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '222',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 666 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 319900,
			        // 价格（单位分）
			        s1: '22',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '333',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 666 // 当前 sku 组合对应的库存
			    },
				
				{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 259900,
			        // 价格（单位分）
			        s1: '33',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '111',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 333 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 279900,
			        // 价格（单位分）
			        s1: '33',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '222',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 309900,
			        // 价格（单位分）
			        s1: '33',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '333',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    },
				
				{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 259900,
			        // 价格（单位分）
			        s1: '44',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '111',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 333 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 279900,
			        // 价格（单位分）
			        s1: '44',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '222',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    },{
			        id: 2259,
			        // skuId，下单时后端需要
			        price: 309900,
			        // 价格（单位分）
			        s1: '44',
			        // 规格类目 k_s 为 s1 的对应规格值 id
			        s2: '333',
			        // 规格类目 k_s 为 s2 的对应规格值 id
			        s3: '0',
			        // 最多包含3个规格值，为0表示不存在该规格
			        stock_num: 444 // 当前 sku 组合对应的库存
			    }],
			    price: '2599',
			    // 默认价格（单位元）
			    stock_num: 227,
			    // 商品总库存
			    collection_id: 2261,
			    // 无规格商品 skuId 取 collection_id，否则取所选 sku 组合对应的 id
			    none_sku: false,
			    // 是否无规格商品
			    hide_stock: false, // 是否隐藏剩余库存
				messages: [
					{
						// 商品留言
						datetime: '0', // 留言类型为 time 时，是否含日期。'1' 表示包含
						multiple: '0', // 留言类型为 text 时，是否多行文本。'1' 表示多行
						name: '留言1', // 留言名称
						type: 'text', // 留言类型，可选: id_no（身份证）, text, tel, date, time, email
						required: '1', // 是否必填 '1' 表示必填
						placeholder: '' // 可选值，占位文本
					},{
						// 商品留言
						datetime: '0', // 留言类型为 time 时，是否含日期。'1' 表示包含
						multiple: '0', // 留言类型为 text 时，是否多行文本。'1' 表示多行
						name: '留言2', // 留言名称
						type: 'date', // 留言类型，可选: id_no（身份证）, text, tel, date, time, email
						required: '1', // 是否必填 '1' 表示必填
						placeholder: '' // 可选值，占位文本
					}
				]
			}
		}
	},
	created(){
		// console.log( this.$route.params.goodsInfo.goods_name );
		this.goodsName = this.$route.params.goodsInfo.goods_name;
		this.goods_img = this.$route.params.goodsInfo.goods_img;
		this.goods_price = this.$route.params.goodsInfo.goods_price;

		// sku商品缩略图
		this.goods.picture = this.$route.params.goodsInfo.goods_img;
	},
	methods: {
		onClickIcon() {
			console.log('点击客户图标');
		},
		// sku组件显示
		onClickButton() {
			// console.log('点击按钮');
			this.show = true;

		},
		gotoHome(){
			this.$router.push('/');
		},
		// sku立即购买
		onBuyClicked( _skuData ){
			console.log( _skuData );
			console.log( this.uploader_img_arr )
		},
		// 图片上传
		afterRead( _file ) {
			// 此时可以自行将文件上传至服务器
			// console.log( _file );
			this.uploader_img_arr.push( _file );
		}
	}
}
</script>

<style scoped>
.goods_detail{}
.goods_detail img{width: 100%;}


/* 导航栏 */
.navBarClass .van-nav-bar__content{
	height:87px;
}


/* 底部按钮 */
.van-goods-action{
	bottom:.5rem;
}
</style>