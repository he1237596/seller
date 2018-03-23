<template>
	<div class="star" :class="starType">
		<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
		<!--<img src="../../../src/components/star/star36_half@2x.png" alt="" />-->
	</div>
</template>

<script>

	const LENGTH = 5;
	const CLS_ON = 'on';
	const CLS_OFF = 'off';
	const CLS_HALF = 'half';
	
	export default{
		props:{
			size: Number,
			score: Number
		},
		data () {
			return {

			}
		},
		computed:{
			starType(){
				return 'star-' + this.size;
			},
			itemClasses(){
				console.log(this.size,this.score)
				let result = [];
				let score = Math.floor(this.score*2)/2;
				let hasDecimal = score%1 !== 0;
				let num = Math.floor(this.score);
				for(let i=0;i<num;i++){
					result.push(CLS_ON);
				}
				if(hasDecimal){
					result.push(CLS_HALF);
				}
				console.log(result.length)
				while(result.length<LENGTH){
					result.push(CLS_OFF);
				}
				console.log(result)
				return result;
			}
		}
	}
</script>

<style>
	.star{
		font-size: 0;
	}
	.star-item{
		display: inline-block;
		background-repeat: no-repeat;
	}
	.star-item:last-child{
		margin-right: 0;
	}
	.star-48>.star-item{
		width: 20px;
		height: 20px;
		margin-right: 22px;
		background-size: 20px 20px;
	}
	.star-48 .on{
		background: url("star48_on@2x.png");
	}
	.star-48 .off{
		background: url('star48_off@2x.png');
	}
	.star-48 .half{
		background: url('star48_half@2x.png');
	}
	.star-48>.star-item{
		width: 20px;
		height: 20px;
		margin-right: 22px;
		background-size: 20px 20px;
	}
	.star-36.on{
		background-image: url('star36_on@2x.png');
	}
	.star-36.off{
		background-image: url('star36_off@2x.png');
	}
	.star-36.half{
		background-image: url('star36_half@2x.png');
	}
	.star-36>.star-item{
		width: 15px;
		height: 15px;
		margin-right: 6px;
		background-size: 15px 15px;
	}
	.star-24.on{
		background-image: url('star24_on@2x.png');
	}
	.star-24.off{
		background-image: url('star24_off@2x.png');
	}
	.star-24.half{
		background-image: url('star24_half@2x.png');
	}
	.star-24>.star-item{
		width: 10px;
		height: 10px;
		margin-right: 3px;
		background-size: 10px 10px;
	}

</style>