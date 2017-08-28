<style lang='scss'>
	.location{
		width:100%;
		min-height:100%;
		position:relative;
		overflow: hidden;
		a{
			color:#ff0021;
		}
		dl{
			position:absolute;
			width:100%;
			text-align: center;
			left: 50%;
			top:40%;
			transform:translate(-50%,-50%);
			dt{
				margin-top:10px;
				font-size:18px;
			}
		}
		.hide{
			visibility:hidden;
		}
		.list{
			width:100%;
			font-size:12px;
			color:#666;
			border-top:1px solid #ccc;
			th{
				font-weight: normal;
				text-align: left;
				border-left:1px solid #ccc;
				border-bottom:1px solid #ccc;
				padding:4px 10px;
			}
			th:first-child{
				border-left:none;
			}
			td{
				padding:4px 10px;
				a{
					color:#000;
				}
			}
			.w300{
				max-width:250px;
				white-space: nowrap;
				overflow: hidden;
				text-overflow:ellipsis;
			}
			.w160{
				max-width:160px;
				white-space: nowrap;
				overflow: hidden;
				text-overflow:ellipsis;
			}
			.w190{
				max-width:190px;
				white-space: nowrap;
				overflow: hidden;
				text-overflow:ellipsis;
			}
			tr:nth-child(even){
				background: #f5f5f7;
			}
			.weight{
				font-weight: bold;
			}
			.playing{
				overflow: hidden;
				text-indent: -999px;
				background: url(http://demo.mikoshu.me/player/icon-playing.png) no-repeat 10px 5px ;
			}
		}
		.loc-head{
			padding:20px 10px 5px 10px;
			h3{
				font-weight: normal;
				font-size:20px;
				display: inline-block;
			}
		}
		.add-folder{
			float:right;
			font-size:12px;
			margin-top:5px;
			color:#666;
			img{
				margin-right:4px;
				vertical-align: -2px;
			}
		}
	}
.pop{
	position:fixed;
	padding:20px 30px 40px;
	left:50%;
	top:50%;
	width:500px;
	max-height:300px;
	transform:translate(-50%,-50%);
	background:#fff;
	border-radius:10px;
	border:1px solid #eee;
	overflow: auto;
	.pop-close{
		position:absolute;
		right:5px;
		top:5px;
		width:18px;
		height:18px;
		color:#999;
		font-size:18px;
		&:hover{
			color:#000;
		}
	}
	.pop-content{
		font-size:14px;
		color:#666;
		line-height: 25px;
		ul{

			li{
				padding:0 10px;
				font-size:12px;
				color:#999;
				border-bottom:1px dashed #999 ;
				span{
					float:left;
					display: block;
					max-width:370px;
					white-space: nowrap;
					overflow: hidden;
					text-overflow:ellipsis;
				}
			}
			li:last-child{
				border:none;
			}
		}
		.add-more{
			position: absolute;
			right:30px;
			bottom:8px;

			font-size:12px;
			float: right;
		}

	}
}

</style>

<template>
	<div class="location">
		<div v-if="popOpen" transition="fade" transition-mode="out-in" class="pop">
			<a href="javascript:;" class="pop-close" v-on:click="closePop" >X</a>
			<div class="pop-content">
				<p>当前文件加载目录为：</p>
				<ul>
					<li v-for="(index,val) in folderList" class="clearfix" >
						<span>{{val}}</span>
						<a href="#" v-on:click="removeFolder" :data-index="index" class="fr">移除</a>
					</li>
				</ul>
				<a href="#" class="add-more" v-on:click="chooseFolder" >继续添加目录</a>
			</div>
		</div>
		<div v-show="hide" >
			<dl>
				<dd><a v-on:click="chooseFolder" href="javascript:;"><img src="http://demo.mikoshu.me/player/folder.png" alt=""></a></dd>
				<dt>还木有载入本地文件哦！<a v-on:click="chooseFolder" href="javascript:;">点击此处</a> 选择文件夹载入！</dt>
			</dl>
			<input type="file" class="hide" v-on:change="choosed" ref="file" nwdirectory >
		</div>
		<div v-show="!hide">
			<div class="loc-head">
				<h3>本地音乐</h3>
				<a class="add-folder" v-on:click="openPop" href="javascript:;"><img src="http://demo.mikoshu.me/player/folder.png" title="管理文件夹" width="20" alt=""></a>
			</div>
			<table class="list">
				<tr>
					<th></th>
					<th>音乐标题</th>
					<th>歌手</th>
					<th>专辑</th>
					<th>时长</th>
					<th>大小</th>
				</tr>
				<tr v-show="hasMusic" v-for="(val,index) in musicList" >
					<td v-bind:class='[index == musicData.currentIndex ? "playing" : ""]'>{{index+1}}</td>
					<td class="w190"><a href="javascript:;" :title="val.name" v-bind:class='[index == musicData.currentIndex ? "weight" : ""]' :data-index="index" :data-link="val.link" :data-singer="val.singer" v-on:click="play">{{val.name}}</a></td>
					<td class="w300" :title="val.singer">{{val.singer}}</td>
					<td class="w160" :title="val.ep">{{val.ep}}</td>
					<td>{{val.duration}}</td>
					<td>{{val.size}}</td>
				</tr>
				<tr v-show="!hasMusic">
					<td colspan="6" style="text-align:center; font-size:18px;">该文件夹里木有音乐，请<a v-on:click="chooseFolder" href="javascript:;">点击此处</a>重新选择文件夹！</td>
				</tr>
			</table>
		</div>
		
	</div>
</template>

<script>
	import fs from 'fs';
	import path from 'path';
	import mm from 'musicmetadata';

	export default {
		props:['musicData'],
		data: function(){
			return {
				musicList: [],
				hide: true,
				folderList: [],
				hasMusic: false,
				hasSrc: false,
				popOpen: false
			};
		},
		methods:{
			chooseFolder: function(){
				this.$refs.file.click()
			},
			choosed: function(e){ // 选择文件夹
				var dirname = e.target.value;
				if(dirname != ''){
					if(this.folderList.length > 0){
						this.hasSrc = false;
						this.folderList.forEach(function(val){
							if(val == dirname){
								alert('该目录已存在，请勿重复添加');
								e.target.value = "";
								this.hasSrc = true;
							}
						}.bind(this))
						if(!this.hasSrc){
							this.folderList.push(dirname);
						}
					}else{
						this.folderList.push(dirname);
					}
				}
				
				e.target.value = '';
				
				this.readList();
			},
			readList: function(){ // 读取localstorage 文件夹数组
				localStorage.folderList = this.folderList;
				if(localStorage.folderList != ''){
					localStorage.folderList.split(",").forEach(function(val){
						this.musicList = [];
						this.readFile(val);
					}.bind(this))
				}else{
					this.hide = true;
				}
				
			},
			readFile: function(dirname){ // 读取路径并且递归文件夹
				var self = this;
				var dir = fs.readdirSync(dirname);
				dir.forEach(function(val){
					val = path.join(dirname,val);
					fs.stat(val,function(err,stat){
						if(err){
							throw err
						}else{
							if(stat.isDirectory()){
								self.readFile(val,dir)
							}else{
								// https://github.com/leetreveil/musicmetadata
								var ext = path.extname(val); // 判断文件是否为音频
								if(ext == ".mp3" || ext == ".wav" || ext == ".wma" || ext == ".ogg" || ext == ".ape" || ext == ".acc" || ext == ".m4a"){
									var parser = mm(fs.createReadStream(val),{ duration: true }, function (err, data) {
									  if (err) throw err;
										var name = data.title;
										var singer = data.artist.toString();
										var size = (parseInt(stat.size)/1024/1024).toFixed(1) + "MB";
										var ep = data.album == ''? '未知':data.album;
										var duration = parseInt(data.duration); // 计算音频时长
										var h = parseInt(duration/3600) > 0 ? parseInt(duration/3600) : '';
										var m = parseInt(duration/60) >10 ? parseInt(duration/60) : '0'+ parseInt(duration/60) ;
										var s = duration%60 > 10 ? duration%60 : '0'+duration%60 ;
										duration = h > 0? h+':'+m+':'+s : m+':'+s; 
										self.hasMusic = true;
										self.musicList.push({name:name,singer:singer,link:val,size:size,ep:ep,duration:duration})
									});
									
								}
							}
						}
					})
				})
				this.hide = false;
			},
			play: function(e){ // 播放
				var link = e.target.getAttribute('data-link');
				var name = e.target.innerHTML;
				var singer = e.target.getAttribute('data-singer');
				this.$emit('change',{
					currentSong:name,
		            currentLink: link,
		            playList:this.musicList,
		            singer:singer,
		            currentIndex:parseInt(e.target.getAttribute('data-index')),
		            singerPic: '',
		            loop: false,
		            idList: [],
		            idIndex: -1,
		            songId: ''
				})
			},
			openPop: function(){ // 打开弹窗
				this.popOpen = true;
			},
			removeFolder: function(e){ // 删除文件夹路径
				var index = e.target.getAttribute("data-index");
				this.folderList.splice(index,1);
				localStorage.folderList = this.folderList;
			},
			closePop: function(){// 关闭弹窗
				this.popOpen = false;
				setTimeout(function(){this.readList();}.bind(this),500);
			}
		},
		mounted: function(){
			if(localStorage.folderList){ // 读取文件列表
				this.folderList = localStorage.folderList.split(",");
			}
			if(this.folderList.length == 0){
				this.hide = true;
			}else{
				this.hide = false;
				this.folderList.forEach(function(val){ //遍历文件夹读取音频文件
					this.readFile(val)
				}.bind(this))
			}
		}
	}
</script>