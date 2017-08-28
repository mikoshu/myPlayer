<style lang="scss">
	.max-long-250{
		max-width:250px;
		overflow: hidden;
		white-space: nowrap;
		text-overflow:ellipsis;
	}
	.max-long-200{
		max-width:200px;
		overflow: hidden;
		white-space: nowrap;
		text-overflow:ellipsis;
	}
	.w60{
		width:70px;
		
		img{
			height:13px;
		}
	}
	.download{
		opacity:.5;
		&:hover{
			opacity:1;
		}
	}
	.location{
		.list{
			em{
				color:red;
				font-style: normal;
			}
		}
		.heart{
			display:inline-block;
			margin-right:10px;
			vertical-align: -2px;
			width:14px;
			height:14px;
			background:url(http://demo.mikoshu.me/player/icon-heart.png) no-repeat;
			background-size: 100% 100%;
			opacity:.6;
		}
		.heart-a{
			display:inline-block;
			margin-right:10px;
			vertical-align: -2px;
			width:14px;
			height:14px;
			background:url(http://demo.mikoshu.me/player/icon-heart-a.png) no-repeat;
			background-size: 100% 100%;
		}
		
	}
	.dn{
		display:none;
	}
	
</style>

<template>
	<div class="location">
		<div class="loc-head">
			<h3>搜索结果</h3>
		</div>
		<table class="list">
			<tr>
				<th></th>
				<th>操作</th>
				<th>音乐标题</th>
				<th>歌手</th>
				<th>专辑</th>
			</tr>
			<tr v-bind:class='[!notFounded ? "":"dn"]' v-for="(val,index) in list" >
				<td v-bind:class='[index == currentIndex2 ? "playing" : ""]'>{{((page-1)*size)+index+1}}</td>
				<td class="w60" >
					<a href="#" v-bind:class="[favorite[val.songId] ? 'heart-a' : 'heart' ]" :data-id="val.songId" :data-index="index" v-on:click="addFavorite" ></a>
					<a href="javascript:;" class="download" ><img v-on:click="download" :data-id="val.songId" src="http://demo.mikoshu.me/player/icon-download.png"></a>
				</td>
				<td class="max-long-200">
					<a v-bind:class='[index == currentIndex2 ? "weight" : ""]' :data-index="index" :data-id="val.songId" href="javascript:;" v-on:click.stop="player" v-html="val.name" ></a>
				</td>
				<td class="max-long-250" :title="val.singer" v-html="val.singer"></td>
				<td class="max-long-200" :title="val.ep" v-html="val.ep" ></td>
			</tr>
			<tr v-bind:class='[notFounded ? "":"dn"]'>
				<td colspan="5" style="text-align:center;">查询无结果或网络不给力，请重试~</td>
			</tr>
		</table>
		<my-page :current-page="page" :total="total" :page-num="size" @change="change"  ></my-page>
	</div>
</template>

<script>
	import pagination from './page.vue';
	export default {
		props:['musicData','query'],
		data: function(){
			return {
				list:[],
				size: 30,
				notFounded: false,
				page:1,
				currentIndex2: -1,
				total: 0,
				favorite:{}
			}
		},
		methods:{
			search: function(){
				var self = this;
				this.list = [];
				fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.search.common&format=json&query='+this.query+'&page_size='+this.size+'&page_no='+this.page,{
					method: 'GET'
				}).then(function(resp){
					return resp.json();
				}).then(function(json){
					var resp = json;
					self.total = parseInt(json.pages.total);
					if(json.song_list){
						self.notFounded = false;
						json.song_list.map(function(val,index){
							self.list.push({
								singer: val.author,
								name: val.title,
								ep: val.album_title == ''? '未知':val.album_title,
								songId : val.song_id
							})
						})
					}else{
						self.notFounded = true;
					}
				})
			},
			player: function(e){
				var self = this;
				var id = e.target.getAttribute("data-id");
				this.currentIndex2 = e.target.getAttribute("data-index");
				this.currentIndex = -1;
				if(id == undefined){
					id = e.target.parentNode.getAttribute("data-id")
					this.currentIndex2 = e.target.parentNode.getAttribute("data-index");
				}
				fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.song.playAAC&songid='+id,{
					method: 'GET'
				}).then(function(resp){
					return resp.json();
				}).then(function(json){
					var resp = json;
					self.$emit('change',{
						currentSong:resp.songinfo.title,
			            currentLink: 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link,
			            playList:self.musicData.playList,
			            singer:resp.songinfo.author,
			            currentIndex:-1,
			            singerPic: 'http://localhost:8081/api/?tourl='+resp.songinfo.pic_small,
			            loop: true,
			            idList: self.musicData.idList,
			            idIndex: self.musicData.idIndex,
			            songId: self.musicData.songId
					})
				})
			},
			download: function(e){
				var self = this;
				var id = e.target.getAttribute("data-id");
				fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.song.playAAC&songid='+id,{
					method: 'GET'
				}).then(function(resp){
					return resp.json();
				}).then(function(json){
					var resp = json;
					//console.log(resp.bitrate.file_link)
					if(resp.bitrate.file_link){
						window.location.href = 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link
					}else{
						alert("无法下载该歌曲")
					}
				})
			},
			addFavorite: function(e){
				var className = e.target.getAttribute('class');
				if(className == 'heart'){
					var id = e.target.getAttribute('data-id');
					var index = e.target.getAttribute('data-index');
					e.target.setAttribute('class','heart-a');
					this.favorite[id] = this.list[index];
					localStorage.favorite = JSON.stringify(this.favorite);
				}
			},
			change(obj){
				this.page = obj.currentPage
			}
		},
		components:{
			"my-page": pagination
		},
		watch:{
			query: function(){
				this.search();
				this.currentIndex2 = -1;
			},
			page: function(){
				this.search()
			}
		},
		mounted: function(){
			
			this.search();
			if(localStorage.favorite){
				this.favorite = JSON.parse(localStorage.favorite);
			}
		}
	}
</script>