<style lang="scss">
  body, div, span, object, input, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, samp, small, strong, sub, sup, tt, var, b, i, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, hr { padding: 0; margin: 0; } 
table { border-collapse: collapse; border-spacing: 0; } 
caption{ text-align:left;} 
input,select{ vertical-align:middle;} 
input,textarea,select{ font:12px Arial, Helvetica, sans-serif; } 
fieldset, img { border: 0; } 
/*address,code,caption,th,cite,dfn,em,var{font-style:normal;} */
ol, ul { list-style: none; } 
h1, h2, h3, h4, h5, h6 { font-size: 100%; } 
q:before, q:after { content:""; } 
legend{ display:none;} 
a{text-decoration: none;}
html,body{
  font-family:"微软雅黑";
  height:calc(100% - 50px );
    -webkit-user-select: none;
}
* { -webkit-box-sizing: border-box; -moz-box-sizing: border-box; box-sizing: border-box;zoom:1.000001; }
/* float */
.clearfix:after{ content:""; height:0; visibility:hidden; display:block; clear:both;}
.clearfix{ zoom:1;}
.clear { clear:both;}
.fl{float:left;}
.fr{float:right;}

$color-r:#c62f2f;
::-webkit-scrollbar {
   width: 8px;
   background:rgab(0,0,0,.5);
}
::-webkit-scrollbar-track {
   -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3); 
   //-webkit-border-radius: 10px;
   //border-radius: 10px;
}
::-webkit-scrollbar-thumb {
   -webkit-border-radius: 10px;
   border-radius: 10px;
   background: rgba(0,0,0,.1); 
}
::-webkit-scrollbar-thumb:window-inactive {
  //background: rgba(169,169,169,0.4); 
}
.top-bar{
  height:50px;
  line-height:50px;
  background:$color-r;
  position:relative;
  -webkit-app-region: drag;
  h1{
    float:left;
    color:#fff;
    padding-left:20px;
    font-size:20px;
  }
  .reload{
    display:block;
    float:left;
    margin-left:60px;
    -webkit-app-region: no-drag;
  }
  .handle-box{
    margin-right:14px;
    padding-top:17px;
    a{
      display:block;
      float:left;
      -webkit-app-region:no-drag;
      opacity:.5;
      &:hover{
        opacity:1;
      }
    }
    .min{
      margin-right:15px;
      width:16px;
      height:9px;
      border-bottom:2px solid #fff;
    }
    .max{
      margin-top:2px;
      margin-right:15px;
      width:16px;
      height:12px;
      border:2px solid #fff;
      border-radius:2px;
    }
    .close{
      width:16px;
      height:16px;
      background:url(http://demo.mikoshu.me/player/icon-close.png) no-repeat;
    }
  }
}
.search-box{
  float:left;
  display:block;
  margin-left:40px;
  padding-top:15px;
  -webkit-app-region: no-drag;
    input{
      display:block;
      width:190px;
      line-height:24px;
      padding-right:30px;
      border-radius:12px;
      border:none;
      outline:none;
      text-indent:16px;
      color:#c77373;
      background:url(http://demo.mikoshu.me/player/icon-search.png) center right #a82828 no-repeat;
    }
  
}

.main{
  height:calc(100vh - 100px);
}

.side-bar{
  float:left;
  width:200px;
  height:100%;
  border-right:1px solid #e1e1e2;
  background:#f5f5f7;
  position:relative;
  dl{
    line-height:32px;
    font-size:14px;
    dt{
      color:#7d7d7d;
      padding-left:14px;
    }
    dd{
      color:#5c5c5c;
      padding-left:45px;
      position:relative;
      cursor:pointer;
      &:hover{
        color:#000;
      }
      ins{
        position:absolute;
        left:14px;
        top:8px;
        width:24px;
        height:16px;
      }
    }
    .active{
      background:#e6e7ea;
      color:#000;
      padding-left:41px;
      border-left:4px solid $color-r;
      ins{
        margin-left:-4px;
      }
    }
    .icon-music{
      left:18px;
      background:url(http://demo.mikoshu.me/player/icon-music.png) no-repeat;
    }
    .icon-heart{
      left:20px;
      background:url(http://demo.mikoshu.me/player/icon-heart.png) no-repeat;
    }
    .icon-history{
      left:20px;
      top:7px;
      background:url(http://demo.mikoshu.me/player/icon-history.png) no-repeat;
    }
  }
  .msg-box{
    position:absolute;
    height:74px;
    width:100%;
    left:0px;
    bottom:0px;
    border-top:1px solid #ccc;
    padding:5px 10px;
    h3{
      width:100%;
      overflow:hidden;
      white-space:nowrap;
      text-overflow:ellipsis;
      font-weight:normal;
      font-size:16px;
    }
    p{
      width:100%;
      overflow:hidden;
      white-space:nowrap;
      text-overflow:ellipsis;
      font-size:12px;
    }
    div{
      width:105px;
      padding-top:12px;
      margin-left:10px;
    }
  }
}
.content{
  float:left;
  width:calc(100% - 200px);
  height:100%;
  background:#fafafa;
  position:relative;
  overflow:auto;
  .fade-transition {
      transition: opacity .3s ease;
  }

  .fade-enter,
  .fade-leave {
      opacity: 0;
  }
}

.footer{
  position:fixed;
  left:0;
  bottom:0px;
  width:100%;
  height:50px;
  border-top:1px solid #ccc;
  background:#fafafa;
  .btn-box{
    padding:8px 14px;
    a{
      display:block;
      float:left;
      width:32px;
      height:32px;
      background:url(http://demo.mikoshu.me/player/icons.png) no-repeat;
      opacity:1;
      margin-left:18px;
      &:hover{
        opacity:.9;
      }
    }
    .btn-last{
      
    }
    .btn-play{
      background-position:-42px 0;
    }
    .btn-pause{
      background-position:-84px 0;
    }
    .btn-next{
      background-position:-128px 0;
    }
  }
  .audio-box{
    width:560px;
    height:50px;
    padding-top:8px;
    padding-left:30px;
    margin-left:25px;
    overflow:hidden;
    audio{
      margin-left:-54px;
      width:560px;
    }
  }
}
</style>
<template>
<div id="app">
  <div class="top-bar">
    <h1>我的音乐播放器</h1>
    <form action="#" class="search-box" v-on:submit.prevent="search" >
      <input type="text" ref="input" placeholder="搜音乐，歌手，歌词..." >
    </form>
    <a href="javascript:;" class="reload" v-on:click="reload" >
      <img src="./images/icon-refresh.png" alt="">
    </a>
    <div class="handle-box fr">
      <a class="min" v-on:click="minimize" href="javascript:;"></a>
      <a class="max" v-on:click="maximize" href="javascript:;"></a>
      <a class="close" v-on:click="close" href="javascript:;"></a>
    </div>
  </div>
  <div class="main clearfix">
    <div class="side-bar">
      <dl>
        <dt>推荐</dt>
        <dd v-on:click="toLocation" v-bind:class='[tab == "location" ? "active" : "" ]'>
          <ins class="icon-music"></ins>本地音乐
        </dd>
        <dd v-on:click="toFavorite" v-bind:class='[tab == "myFavorite" ? "active" : "" ]'>
          <ins class="icon-heart"></ins>我喜欢的音乐
        </dd>
        <dd><ins class="icon-history"></ins>最近播放</dd>
      </dl>
      <div class="msg-box">
        <img v-if="!musicData.singerPic" src="./images/img-music.png" class="fl" alt="">
        <img v-if="musicData.singerPic" v-bind:src="musicData.singerPic" width="64" height="64" class="fl" alt="">
        <div class="fl">
          <h3>{{musicData.currentSong}}</h3>
          <p>{{musicData.singer}}</p>
        </div>
      </div>
    </div>
    <div class="content">
      <component :is="tab" :music-data="musicData" :query="query" transition="fade" transition-mode="out-in" @change="change"></component>
    </div>
    <div class="footer">
      <div class="btn-box clearfix fl">
        <a href="javascript:;" v-on:click="pre" class="btn-last"></a>
        <a href="javascript:;" v-bind:class="[isPlay ? 'btn-pause' : 'btn-play' ]" v-on:click="togglePlay" ></a>
        <a href="javascript:;" v-on:click="next" class="btn-next"></a>
      </div>
      <div class="audio-box fl">
        <audio ref="audio" v-bind:src="musicData.currentLink" autoplay="autoplay" v-on:ended="next" controls="controls"></audio>
      </div>
    </div>
  </div>
</div>
</template>
<script>
import location from './components/location.vue';
import myFavorite from './components/myFavorite.vue';
import search from './components/searchResult.vue';
import './server.js'
  export default{
    data(){
      return {
        views: 'testttt',
        tab: 'location',
        musicData:{
          currentSong:'',
          currentLink: '',
          playList:[],
          singer:'',
          currentIndex:-1,
          singerPic: '',
          loop: false,
          idList: [],
          idIndex: 0,
          songId: ''
        },
        isPlay: false,
        isCircle: false,
        isMax: false,
        query: '',
      }
    },
    methods:{
      togglePlay: function(){
        if(this.$refs.audio.getAttribute('src') == ""){
          alert("请先选择一首音乐再播放哦~")
          return false
        }
        if(this.isPlay){
          this.$refs.audio.pause();
          this.isPlay = false;
        }else{
          this.$refs.audio.play();
          this.isPlay = true;
        }
      },
      pre: function(){
        var self = this;
        if(!!~this.musicData.idIndex ){
          if(this.musicData.idIndex > 0){
            this.musicData.idIndex -- ;
            var id = this.musicData.idList[this.musicData.idIndex];
            fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.song.playAAC&songid='+id,{
              method: 'GET'
            }).then(function(resp){
              return resp.json();
            }).then(function(json){
              var resp = json;
              if(resp.error_code == '22000'){
                self.musicData.currentLink = 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link;
                self.musicData.songId = resp.songinfo.song_id;
                self.musicData.currentSong = resp.songinfo.title;
                self.musicData.singer = resp.songinfo.author;
                self.musicData.singerPic = 'http://localhost:8081/api/?tourl='+resp.songinfo.pic_small;
              }else{
                alert("网络错误！");
                return false;
              }
              
            })
          }else{
            alert("已经是第一首歌了哦~~");
            return false;
          }
        }else{
          var index = parseInt(this.musicData.currentIndex);
          if(index > 0){
            index -- ;
            this.musicData.currentIndex = index;
            this.musicData.currentSong = this.musicData.playList[index].name;
            this.musicData.currentLink = this.musicData.playList[index].link;
            this.musicData.singer = this.musicData.playList[index].singer;
          }else{
            alert("已经是第一首歌了哦~~");
            return false;
          }
        }
      },
      next: function(){
        var self = this;
        if(!!~this.musicData.idIndex ){
          var len = this.musicData.idList.length - 1;
          if(this.musicData.idIndex < len){
            this.musicData.idIndex ++ ;
            var id = this.musicData.idList[this.musicData.idIndex];
            fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?from=qianqian&version=2.1.0&method=baidu.ting.song.playAAC&songid='+id,{
              method: 'GET'
            }).then(function(resp){
              return resp.json();
            }).then(function(json){
              var resp = json;
              if(resp.error_code == '22000'){
                self.musicData.currentLink = 'http://localhost:8081/api/?tourl='+resp.bitrate.file_link;
                self.musicData.songId = resp.songinfo.song_id;
                self.musicData.currentSong = resp.songinfo.title;
                self.musicData.singer = resp.songinfo.author;
                self.musicData.singerPic = 'http://localhost:8081/api/?tourl='+resp.songinfo.pic_small;
              }else{
                alert("网络错误！");
                return false;
              }
              
            })
          }else{
            alert("已经是最后首歌了哦~~");
            return false;
          }
        }else{
          var index = parseInt(this.musicData.currentIndex);
          var len = this.musicData.playList.length - 1;
          if(index < len){
            index ++ ;
            this.musicData.currentIndex = index;
            this.musicData.currentSong = this.musicData.playList[index].name;
            this.musicData.currentLink = this.musicData.playList[index].link;
            this.musicData.singer = this.musicData.playList[index].singer;
          }else{
            alert("已经是最后首歌了哦~~");
            return false;
          }
        }
      },
      minimize: function(){
        var win = nw.Window.get();
        win.minimize()
      },
      maximize: function(){
        var win = nw.Window.get();
        if(this.isMax){
          win.restore()
          this.isMax = false;
        }else{
          win.maximize()
          this.isMax = true;
        }
      },
      close: function(){
        var win = nw.Window.get();
        win.close()
      },
      reload: function(){
        window.location.reload()
      },
      toLocation: function(){
        this.tab = 'location';
      },
      toFavorite: function(){
        this.tab = 'myFavorite';
      },
      search: function(){ // 搜索歌曲
        this.query = this.$refs.input.value;
        this.tab = 'search';
      },
      change(obj){
        this.musicData = obj;
      }
    },
    watch: {
      musicData: function(){
        this.isPlay = true;
      },
      loop: function(){
        this.$refs.audio.loop = this.musicData.loop;
      }
    },
    components:{
      'location': location,
      'myFavorite': myFavorite,
      'search': search
    },
    compiled: function(){

    }
  }
</script>