<template>
	<div>
		<el-container>
			<el-header class="header-box">
				<div class="to-index-box" @click="tomerchants"><i class="to-index el-icon-arrow-left"></i></div>
				<h3>{{name}}</h3>
			</el-header>
			<el-main class="goods-box">
				<el-row>
					<el-col :span="24" class="table-h">
						<el-row>
							<el-col :span="8">商品名称</el-col>
							<el-col :span="4">数量</el-col>
							<el-col :span="6">价格</el-col>
							<el-col :span="6">操作</el-col>
						</el-row>
					</el-col>
					<el-col :span="24">
						<div class="goods-col" v-for="(val,key) in goodslist.datalist" v-if="val.num">
							<el-row>
								<el-col :span="8" class="goods-name">{{val.goodsname}}</el-col>
								<el-col :span="4" class="goods-num">{{val.num}}</el-col>
								<el-col :span="6" class="goods-prices">{{val.price}}</el-col>
								<el-col :span="6" class="operate-btn">
									<i class="el-icon-remove change-btn" @click="reducenum(key)"></i>
									<i class="el-icon-circle-plus change-btn" @click="addnum(key)"></i>
								</el-col>
							</el-row>
						</div>
					</el-col>
				</el-row>
			</el-main>
			<div class="payment_btn" @click="dialogVisible = true">
				加入购物车
			</div>
			<el-dialog
			  title=""
			  :visible.sync="dialogVisible"
			  :close-on-click-modal=false
			  width="80%"
			  :before-close="closemodel">
			  <span>是否加入购物车？</span>
			  <span slot="footer" class="dialog-footer">
			    <el-button @click="dialogVisible = false">取 消</el-button>
			    <el-button type="primary" @click="payment()">确 定</el-button>
			  </span>
			</el-dialog>
		</el-container>
		
	</div>
</template>

<script>
	export default {
		name:"shoppingcart",
		data(){
			return{
				name:"已选商品",
				goodslist:'',
				dialogVisible:false
			}
		},
		created(){// el挂载之前调用
			let $this=this;
			let goods=$this.$store.state.cartlist;
			let sellerId=$this.$route.params.id;// 路由获取id
			if(goods){
				$this.goodslist=goods
			}
			
		},
		methods:{ // 定义方法（逻辑在此处理）
			tomerchants(){
				this.$router.push({path:'/merchant'})
			},
			addnum(index){
				let $this=this;
				let goods=$this.$store.state.cartlist;
				console.log($this.$store.state.order)
				goods.datalist[index].num++
			},
			reducenum(index){
				let $this=this;
				let goods=$this.$store.state.cartlist;
				if(goods.datalist[index].num==0){
					return
				}
				goods.datalist[index].num--
			},
			closemodel(){
				this.dialogVisible=false;
			},
			payment(){
				let $this=this;
				$this.$store.commit("cartlist",null);
				let commitDatalist=[];
				for(var i=0;i<$this.goodslist.datalist.length;i++){
					if($this.goodslist.datalist[i].num>0){
						commitDatalist.push($this.goodslist.datalist[i]);
					}
				};
				$this.$store.commit("addGoods",{
					cartname:$this.goodslist.cartname,
					datalist:commitDatalist
				});
				$this.goodslist=[]
				$this.dialogVisible=false;
			}
		}
	}
</script>

<style scoped>
	.header-box{
		background-image: -webkit-gradient(linear, left top, right top, from(#0af), to(#0085ff));
		color: #ffffff;
		height: 60px;
	}
	.to-index-box{
		float: left;
		height: 100%;
		
	}
	.to-index{
		font-size: 24px;
		font-weight: 600;
		line-height: 60px;
		
	}
	.header-box>h3{
		text-align: center;
		width: auto;
		display: inline-block;
	}
	.table-h{
		border-bottom: 1px solid #e6e6e6;
		padding: 10px;
		font-size: 16px;
		font-weight:600;
		color: rgb(0,133,255);
	}
	.goods-box{
		padding: 0px;
	}
	.goods-col{
		padding: 20px 10px;
		height: 30px;
		line-height: 30px;
		border-bottom: 1px solid #e6e6e6;
	}
	.goods-name,.goods-num,.goods-prices{
		color: red;
		font-size: 16px;
		font-weight: 600;
	}
	.change-btn{
		font-size: 26px;
		color: rgb(0, 133, 255);
	}
	.payment_btn{
		width: 100%;
		height: 60px;
		background-image: -webkit-gradient(linear, left top, right top, from(#0af), to(#0085ff));
		position: fixed;
		bottom: 0;
		left: 0;
		line-height: 60px;
		font-size: 24px;
		color: #ffffff;
	}
</style>