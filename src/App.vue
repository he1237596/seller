<template>
  <div id="app">	
    <v-header :seller="seller"></v-header>
    <div class="tab">
    	<div class="tab-item">
    		<router-link to="/goods">商品</router-link>
    	</div>
    	<div class="tab-item">
    		<router-link to="/ratings">评论</router-link>
    	</div>
    	<div class="tab-item">
    		<router-link to="/seller">商家</router-link>
    	</div>
    </div>
    <router-view></router-view>
    <!--<div class="content">content</div>-->
  </div>
</template>

<script>
	import header from "./components/header/header.vue"
	const ERR_OK = 0;
	
	export default {
	  name: 'App',
	  data(){
	  	return {
	  		seller:{}
	  	};
	  },
	  created(){
	  	this.axios.get('/api/seller')
		  .then((response) => {
		  	if(response.data.errno === ERR_OK){
		  		this.seller = response.data.data;
		  		console.log(this.seller)
		  	}
		  })
		  .catch(error => console.log(error));
	  },
	  components:{
	  	'v-header': header
	  }
	}
</script>
<style>
	.tab{
		display: flex;
		width: 100%;
		height: 40px;
		line-height: 40px;
		position: relative;
	}
	.tab:after{
		display: block;
		position: absolute;
		left: 0;
		bottom: 0;
		width: 100%;
		border-top: 1px solid rgba(7,17,27,0.1);
		content: " ";
	}
	.tab-item{
		flex: 1;
		text-align: center;
	}
	.tab-item>a{
		display: block;
		font-size: 14px;
		color: rgb(77,85,93);
		text-decoration: none;
	}
	.tab-item>.active{
		color: #f01414;
	}
</style>
