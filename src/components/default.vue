<style lang="scss">
    .myDefault{
        margin:0 auto auto auto;
        //max-width:950px;
        padding:10px 20px 20px;
        .title{
            padding:10px 5px 5px;
            border-bottom:1px solid #ccc;
            margin:0 auto auto auto;
        }
        .music-top{
            dl{
                margin-top:20px;
                text-align:center;
                float:left;
                width:22%;
                margin-right:4%;
                img{
                    width:100%;
                    cursor:pointer;
                }
                &:last-child{
                    margin-right:0;
                }
            }
            dt{
                cursor:pointer;
            }
        }
        .section-fix{
            position:absolute;
            top:0;
            left:0;
            width:100%;
            height:100%;
            z-index:99;
            background:#fff;
        }
    }
    .dn{
        display:none;
    }
</style>

<template>
    <div class="myDefault">
        <h3 class="title">推荐歌单</h3>
        <div class="music-top">
            <dl @click="openList(1)">
                <dd><img src="./../images/new.jpg" alt="" ></dd>
                <dt>新歌榜</dt>
            </dl>
            <dl @click="openList(2)">
                <dd><img src="./../images/hot.jpg" alt="" ></dd>
                <dt >热歌榜</dt>
            </dl>
            <dl @click="openList(16)">
                <dd><img src="./../images/popular.jpg" alt="" ></dd>
                <dt>流行榜</dt>
            </dl>
            <dl @click="openList(11)">
                <dd><img src="./../images/rock.jpg" alt="" ></dd>
                <dt>摇滚榜</dt>
            </dl>
        </div>
        <div class="music-top">
            <dl @click="openList(12)">
                <dd><img src="./../images/jazz.jpg" alt="" ></dd>
                <dt>爵士乐</dt>
            </dl>
            <dl @click="openList(21)">
                <dd><img src="./../images/om.jpg" alt="" ></dd>
                <dt>欧美金曲</dt>
            </dl>
            <dl @click="openList(22)">
                <dd><img src="./../images/old.jpg" alt="" ></dd>
                <dt>经典老歌</dt>
            </dl>
            <dl @click="openList(25)">
                <dd><img src="./../images/net.jpg" alt="" ></dd>
                <dt>网络歌曲</dt>
            </dl>
        </div>
        <div class="section-fix" v-if="showList">
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
                    <tr v-if="!notFonded" v-for="(val,index) in list" >
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
                    <tr v-if="notFounded">
                        <td colspan="5" style="text-align:center;">查询无结果或网络不给力，请重试~</td>
                    </tr>
                </table>
                <my-page :current-page="page" :total="total" :page-num="size" @change="change"  ></my-page>
            </div>
        </div>
    </div>
</template>

<script>
    import List from './list.vue';
    import pagination from './page.vue';
    export default {
        data(){
            return {
                type: '',
                list: [],
                size: 30,
                notFounded: false,
                page:1,
                currentIndex2: -1,
                total: 0,
                favorite:{},
                showList: false
            }
        },
        methods:{
            openList(type){
                var self = this;
                this.type = type;
                fetch('http://tingapi.ting.baidu.com/v1/restserver/ting?method=baidu.ting.billboard.billList&type='+this.type+'&size='+this.size+'&offset='+((this.page-1) * this.size),{
                    method: 'GET'
                }).then(function(resp){
                    return resp.json();
                }).then(function(json){
                    self.total = parseInt(json.billboard.billboard_songnum);
                    self.list = json.song_list;
                    console.log(self.list)
                    self.showList = true;

                })
            },
            change(obj){
                this.page = obj.currentPage
            }
        },
        components:{
            "my-page": pagination
        }
    }
</script>