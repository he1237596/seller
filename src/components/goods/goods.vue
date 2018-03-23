<template>
	<div class="goods">
		<div class="menu-warpper" ref="menuWarpper">
			<ul>
				<li v-for="(item,key) in goods" class="menu-item" :class="{current:currentIndex===key}">
					<span class="text">{{key}}
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-warpper" ref="foodsWarpper">
			<ul>
				<li v-for="item in goods" class="food-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item">
							<div class="icon">
								<img :src="food.icon" width="57" height="57" alt="" />
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">¥{{food.price}}</span>
									<span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>

</template>
<script>
	import BScroll from 'better-scroll';
	const ERR_OK = 0;
	export default {
		name: 'goods',
		data (){
			return {
				goods: [],
				listHeight: [],
				scrollY: 0
			}
		},
		computed:{
			currentIndex(){
				console.log(this.scrollY)
				for(var i=0;i<this.listHeight.length;i++){
					console.log(i)
					let height1 = this.listHeight[i];
					let height2 = this.listHeight[i+1];
					console.log(i)
					console.log(!height2)
					console.log(this.scrollY >= height1 && this.scrollY < height2)
					if(!height2 ||(this.scrollY >= height1 && this.scrollY < height2)){
						console.log(i,111111111)
						return i;
					}
				}
				return 0;
			}
		},
		created (){

			this.classMap = ["decrease","discount","guarantee","invoice","special"];
			this.axios.get('/api/goods')
			.then((response) => {
		  	if(response.data.errno === ERR_OK){
		  		this.goods = response.data.data;
		  		this._initScroll();
		  		this.$nextTick( () => {
				    this.calculateHeight();
				  })
		  		
		  		}
			})
			.catch(error => console.log(error));
		},
		methods: {

			_initScroll(){
				this.menuScroll = new BScroll(this.$refs.menuWarpper,{});
				this.foodsScroll = new BScroll(this.$refs.foodsWarpper,{probeType :3});
				
				this.foodsScroll.on('scroll',(pos) =>{
					this.scrollY = Math.abs(Math.round(pos.y));
				})
//			console.log(this.$refs.menuWarpper)
				
			},
			calculateHeight(){
				let foodList = this.$refs.foodsWarpper.getElementsByClassName('food-list-hook');
				console.log(foodList.length,222222);
				console.log(foodList,1111111)
				console.log(foodList.length,222222);
				let height = 0;
				this.listHeight.push(height);
				
				for(let i=0;i < foodList.length;i++){
					console.log(i)
					let item = foodList[i];
					height += item.clientHeight;
					console.log(foodList[i])
					this.listHeight.push(height);
				}
				console.log(this.listHeight)
				
			}
		}
	}
</script>

<style scoped="scoped"> 
.goods{
	position: absolute;
	bottom: 46px;
	top: 174px;
	width: 100%;
	display: flex;
	overflow: hidden;
}
.menu-warpper{
	flex: 0 0 80px;
	width: 80px;
	background: #f3f5f7;
}
.menu-item{
	display: table;
	height: 54px;
	width: 80px;
	line-height: 14px;
}
.current{
	position: relative;
	z-index: 10;
	margin-top: -1px;
	background: #fff;
	font-weight: 700;
}
.current>.text{
	border: none!important;
}
.menu-item .icon{
		display: inline-block;
		vertical-align: top;
		height: 12px;
		width: 12px;
		margin-right: 2px;
		background-size: 12px 12px;
		background-repeat: no-repeat;
	}
.decrease{
		background-image: url('decrease_3@2x.png');
	}
.discount{
		background-image: url('discount_3@2x.png');
	}
.guarantee{
		background-image: url('guarantee_3@2x.png');
	}
.invoice{
		background-image: url('invoice_3@2x.png');
	}
.special{
		background-image: url('special_3@2x.png');
	}
.menu-item .text{
		display: table-cell;
		font-size: 12px;
		vertical-align: middle;
		width: 56px;
		border-bottom: 1px solid rgba(7,17,27,0.1);
	}
.foods-warpper{
	flex: 1;
}
.foods-warpper .title{
	padding-left: 14px;
	height: 26px;
	line-height: 26px;
	border-left: 2px solid #d9dde1;
	font-size: 12px;
	color: rgb(147,153,159);
	background: #f3f5f7;
}
.food-item{
	display: flex;
	margin: 18px;
	padding-bottom: 18px;
	border-bottom: 1px solid rgba(7,17,27,0.1);
}
.food-item:last-child{
	border-bottom: none;
	margin-bottom: 0;
}
.food-item .icon{
	flex: 0 0 57px;
	margin-right: 10px;
}
.food-item .content{
	flex: 1;
}
.food-item .name{
	margin: 2px 0 8px 0;
	height: 14px;
	font-size: 14px;
	color: rgb(7,17,27);
}
.desc,.extra{

	line-height: 10px;
	font-size: 10px;
	color: rgb(147,153,159);
}
.desc{
	line-height: 12px;
	margin-bottom: 8px;
}
.count{
	margin-right: 12px;
}
.price{
	font-weight: 700;
	line-height: 24px;
}
.price>.now{
	color: red;
}
.old{
	text-decoration: line-through;
	font-size: 10px;
	color: rgb(147,153,159);
}
</style>