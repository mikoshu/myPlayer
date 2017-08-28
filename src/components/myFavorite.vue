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
			<h3>我喜欢的音乐</h3>
		</div>
		<table class="list">
			<tr>
				<th></th>
				<th>操作</th>
				<th>音乐标题</th>
				<th>歌手</th>
				<th>专辑</th>
			</tr>
			<tr v-for="(val,key,index) in favorite" >
				<td v-bind:class='[musicData.songId == key ? "playing" : ""]'>{{index+1}}</td>
				<td class="w60" >
					<a href="javascript:;" v-bind:class="[favorite[val.songId] ? 'heart-a' : 'heart' ]" :data-id="val.songId" v-on:click="removeFavorite" ></a>
					<a href="javascript:;" class="download" ><img v-on:click="download" :data-id="val.songId" src="http://demo.mikoshu.me/player/icon-download.png"></a>
				</td>
				<td class="max-long-200">
					<a v-bind:class='[musicData.songId == key ? "weight" : ""]' :data-id="val.songId" href="javascript:;" :data-index="index" v-on:click.stop="player" v-html="val.name" ></a>
				</td>
				<td class="max-long-250" :title="val.singer" v-html="val.singer" ></td>
				<td class="max-long-200" :title="val.ep" v-html="val.ep"></td>
			</tr>
			<tr v-bind:class='[notFounded ? "":"dn"]'>
				<td colspan="5" style="text-align:center;">暂无喜欢的音乐，快去搜索列表添加吧！</td>
			</tr>
		</table>
	</div>
</template>

<script>
	export default {
		props:['musicData'],
		data: function(){
			return {
				list:[],
				size: 30,
				notFounded: false,
				page:1,
				total: 0,
				favorite:{},
				idList:this.musicData.idList,

			}
		},
		methods:{
			player: function(e){
				var self = this;
				var id = e.target.getAttribute("data-id");
				this.currentIndex = -1;
				for( var k in this.favorite){
					this.idList.push(k);
				}
				this.idIndex = parseInt(e.target.getAttribute("data-index"));
				fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.song.playAAC&songid='+id,{
					method: 'GET'
				}).then(function(resp){
					return resp.json();
				}).then(function(json){
					var resp = json;
					if(resp.error_code == '22000'){
						self.currentLink = 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link;
						self.songId = resp.songinfo.song_id;
						self.currentSong = resp.songinfo.title;
						self.singer = resp.songinfo.author;
						self.singerPic = 'http://localhost:8081/api/?tourl='+resp.songinfo.pic_small;
						self.$emit('change',{
							currentSong:resp.songinfo.title,
					        currentLink: 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link,
					        playList:self.musicData.playList,
					        singer:resp.songinfo.author,
					        currentIndex:-1,
					        singerPic: 'http://localhost:8081/api/?tourl='+resp.songinfo.pic_small,
					        loop: self.musicData.loop,
					        idList: self.idList,
					        idIndex: parseInt(e.target.getAttribute("data-index")),
					        songId: resp.songinfo.song_id
						})
					}else{
						alert("网络错误！")
					}
					
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
			removeFavorite: function(e){
				var className = e.target.getAttribute('class');
				if(className == 'heart-a'){
					var id = e.target.getAttribute('data-id');
					e.target.setAttribute('class','heart');
					var obj = this.favorite;
					delete obj[id];
					this.$delete('favorite');
					this.$set('favorite',obj);
					console.log(this.favorite);
					localStorage.favorite = JSON.stringify(this.favorite);
				}
			}
		},
		watch:{

		},
		mounted: function(){
			if(localStorage.favorite && localStorage.favorite != "{}" ){
				this.favorite = JSON.parse(localStorage.favorite);
			}else{
				this.notFounded = true;
			}
		}
	}
</script>