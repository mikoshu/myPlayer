<style lang="scss">
	.pages{
		text-align: center;
		ul{
			display:inline-block;
			margin: 20px auto;
		}
		
		li:last-child{
			border-radius:0 4px 4px 0;
		}
		li:first-child{
			border-radius:4px 0 0 4px;
		}
		li{
			float:left;
			border:1px solid #ccc;
			margin-left:-1px;
			transition:all .5s;
			&:hover{
				border-color:#c62f2f;
				a{
					background:#c62f2f;
					color:#fff;
				}
			}
			a{
				display:block;
				line-height:24px;
				padding:0 10px;
				font-size:12px;
				color:#666;
				transition:all .5s;
			}
		}
		.active{
			border-color:#c62f2f;
			a{
				background:#c62f2f;
				color:#fff;
			}
		}
	}
</style>

<template>
	<div class="pages">
		<ul>
			<li><a href="javascript:;" v-on:click="pre">上一页</a></li>
			<li v-if="showFirst" :class="[1 == currentPage ? 'active':'']" ><a href="javascript:;"  v-on:click="toPage" >1</a></li>
			<li v-if="showFistDots"> <a href="javascript:;">...</a> </li>
			<li v-if="showLoop" v-for="index in size-2" v-bind:class="[index+1 == currentPage ? 'active':'']" ><a href="javascript:;" v-on:click="toPage" >{{index+1}}</a></li>
			<li v-if="showLoop2" v-for="index in pageNo" v-bind:class="[index+1 == currentPage ? 'active':'']" ><a href="javascript:;" v-on:click="toPage" >{{index+1}}</a></li>


			<li v-if="showLoop3" v-for="index in showCount" :class="[index==3?'active':'']"><a href="javascript:;" v-on:click="toPage" >{{currentPage+(index-3)}}</a></li>


			<li v-if="showLoop4" v-for="index in showCount" :class="[pageNo-(7-index) == currentPage ? 'active':'']" ><a href="javascript:;" v-on:click="toPage" >{{pageNo-(7-index)}}</a></li>

			<li v-if="showLastDots"> <a href="javascript:;">...</a> </li>
			<li v-if="showLast" v-bind:class="[pageNo == currentPage ? 'active':'']"><a href="javascript:;" v-on:click="toPage" >{{pageNo}}</a></li>
			<li><a href="javascript:;" v-on:click="next">下一页</a></li>
		</ul>
	</div>
</template>

<script>
	export default {
		props:{
			currentPage:{
				type: Number
			},
			pageNum:{
				type:Number
			},
			total:{
				type:Number
			}
		},
		data: function(){
			return {
				showCount: 6,
				pageNo: 0,
				showFirst: false,
				showFistDots: false,
				showLast: false,
				showLastDots: false,
				showLoop: false,
				showLoop2: false,
				showLoop3: false,
				showLoop4: false,
				size: 10
			}
		},
		methods:{
			toPage: function(e){
				var no = parseInt(e.target.innerHTML);
				this.$emit('change',{
					currentPage: no
				})
			},
			pre: function(){
				if(this.currentPage > 1){
					this.$emit('change',{
						currentPage: this.currentPage - 1
					})
				}
				
			},
			next: function(){
				if(this.currentPage < this.pageNo){
					this.$emit('change',{
						currentPage: this.currentPage + 1
					})
				}
			},
			init: function(){
				this.pageNo = Math.ceil(this.total/this.pageNum);
				if(this.pageNo < this.size){
					this.showFirst = true;
					this.showLoop = false;
					this.showLastDots = false;
					this.showFistDots = false;
					this.showLast = false;
					this.showLoop2 = true;
					this.showLoop3 = false;
					this.showLoop4 = false;
				}else if(this.pageNo > this.size){
					this.showFirst = true;
					this.showLoop = true;
					this.showLastDots = true;
					this.showFistDots = false;
					this.showLast = true;
					this.showLoop2 = false;
					this.showLoop3 = false;
					this.showLoop4 = false;
				
					if(this.currentPage > this.size-3 && this.currentPage < this.pageNo - 4 ){
						this.showFirst = true;
						this.showLoop = false;
						this.showLastDots = true;
						this.showFistDots = true;
						this.showLast = true;
						this.showLoop2 = false;
						this.showLoop3 = true;
						this.showLoop4 = false;
					}else if(this.currentPage >= this.pageNo - 4){
						this.showFirst = true;
						this.showLoop = false;
						this.showLastDots = false;
						this.showFistDots = true;
						this.showLast = true;
						this.showLoop2 = false;
						this.showLoop3 = false;
						this.showLoop4 = true;
					}
				}
				
			}
		},
		watch:{
			total: function(){
				this.init()
			},
			currentPage: function(){
				this.init()
			}
		},
		mounted: function(){
			this.init()
		}
	}
</script>