<template>
    <div class="app-homeContainer">
        <!--顶部导航条-->
        <mt-header fixed title="逛校园"></mt-header>
         <!--轮播图-->
        <mt-swipe :auto="4000">
        <mt-swipe-item v-for="pic in list" :key="pic.id"> <img :src="pic.img_url"> </mt-swipe-item>
        </mt-swipe>
         <ul class="mui-table-view mui-grid-view mui-grid-8">
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><router-link to="/home/newslist">
		                    <img src="../../img/icon1.png">
		                    <div class="mui-media-body">校内生活</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><router-link to="/home/goodslist">
		                    <img src="../../img/icon2.png">
		                    <div class="mui-media-body">周边生活</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><router-link to="/shop">
		                    <img src="../../img/icon3.png">
		                    <div class="mui-media-body">本校圈子</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><router-link to="/home/photo">
		                    <img src="../../img/icon4.png">
		                    <div class="mui-media-body">校园头条</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><a href="#">
		                    <img src="../../img/icon5.png">
		                    <div class="mui-media-body">技能培训</div></a></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><a href="#">
		                    <img src="../../img/icon6.png">
		                    <div class="mui-media-body">招聘会</div></a></li>
                    <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><a href="#">
		                    <img src="../../img/icon7.png">
		                    <div class="mui-media-body">考研</div></a></li>
                    <li class="mui-table-view-cell mui-media mui-col-xs-3 mui-col-sm-3"><a href="#">
		                    <img src="../../img/icon8.png">
		                    <div class="mui-media-body">校园服务</div></a></li>
		        </ul> 
                
				<div class="mui-card-content-inner">
                    <a href="#"><img src="../../img/msg.png" class="img1">
						2018年教师资格证新手报考指南
                    <img src="../../img/jt.png" class="img2"></a>
			    </div>
                <div class="mui-card">
				<div class="mui-card-content">
                <div class="wid">
                    <a><img src="../../img/adv1.jpg"></a>    
                </div>
                <div class="flex">
                <a><img src="../../img/adv2.jpg"></a>
                <a><img src="../../img/adv3.jpg"> </a>
                </div>
                </div>
                </div>
                <div class="mui-card">
				<div class="mui-card-content">					
						<h3>互动区</h3>
                        <div class="flex">
                        <a src="#"><img src="../../img/adv4.jpg"></a>
                        <a src="#"><img src="../../img/adv5.jpg"></a>
                        <a src="#"><img src="../../img/adv6.png"></a>                      
					</div>
				</div>
			</div>
            
            <div class="mui-card" > 
            <h3>最新活动</h3>
                <a src="#" v-for="item in card"><img :src="item.img_url"></a>
            </div>
            <div class="mui-card">
             <h3>社团介绍</h3>
                <a src="#"><img src="../../img/club1.png"></a>
                 <a src="#"><img src="../../img/club2.png"></a>
            </div>
            <ul class="mui-table-view">
				<li class="mui-table-view-cell mui-media" v-for="c in classes">
					<a href="javascript:;">
						<img class="mui-media-object mui-pull-left" :src="c.img_url">
						<div class="mui-media-body">
							{{c.title}}
							<p class='mui-ellipsis'>￥{{c.price}}</p>
                            <p class='mui-ellipsis'>销量:{{c.point}}</p>
                            <a src="#"><img src="../../img/cart.png"></a>
						</div>
					</a>
				</li>
			</ul>
            <mt-button type="default" size="large" @click="getMore()">加载更多</mt-button>
           
            
    </div>
</template>
<script>
import {Toast} from 'mint-ui'
export default{
data(){
        return{
           list:[],
           card:[],
           lists:[],
           classes:[],
           pageIndex:0,
            pageSize:5,
            hasMore:true,
            pageCount:2,
            }
        
    },
    methods:{
        getImage(){
            this.$http.get("http://localhost:3000/imagelist").then(result=>{
                this.list=result.body;
                
            })
        },
        getCard(){
            this.$http.get("http://localhost:3000/cardPic").then(result=>{
                this.card=result.body;    
            })
        },
        getMore(){
                this.pageIndex++;
                this.hasMore = this.pageIndex<=this.pageCount;
                if(!this.hasMore){Toast('没有更多了');return}
                var url="http://127.0.0.1:3000/classlist";
                url+="?pno="+this.pageIndex+"&pageSize="+this.pageSize;
                this.$http.get(url).then(result=>{
                    var rows=this.lists.concat(result.body.data);
                    this.classes=rows;
                    this.pageCount=result.body.pageCount;
                })
            }
    },
   
    
    created(){
        this.getImage();
        this.getCard();
        this.getMore();
    }

}


</script>
<style>
    .app-homeContainer .mint-header{
        background:#fff;
        color:#000;
    }
    .app-homeContainer .mint-swipe{
        height:176px;
    }
    .app-homeContainer .mint-swipe img{
        width:100%;
    }
   .app-container .mui-grid-view.mui-grid-8{
        background:#fff;
        height:195px;   
    }
    .app-container .mui-grid-view.mui-grid-8 .mui-table-view-cell{
        border:none;
    }
    .wid{
        height:136px;
    }
    .wid img{
        width:100%;
        height:100%;
    }
    .flex{
        display:flex;
        
    }
    .flex a img{
        width:100%;
        height:100%;
    }
    .mui-table-view li img{
        width:40px;
        height:40px;
    }
    .mui-card-content-inner{
        width:100%;
        padding:15px 0;
        background:#fff;
        margin-top:15px;
       
    }
    .mui-card-content-inner a{
    color:#353535;
    }
    .mui-card-content-inner .img1{
        width:20px;
        height:20px;
    }
    .mui-card-content-inner .img2{
        width:12px;
        height:12px;
        float:right;
        margin-top:10px;
    }
    .app-homeContainer .mui-card{
        margin:10px 2px;
    }
     h3{
        text-align:center;
        font-size:18px;
        color:#ACACAC;
    }
    .mui-card-content{
        height:120px;
    }
    .mui-card-content .flex{
        justify-content:space-between;
    } 
    .mui-media-body a img{
        width:18px;
        height:18px;
        float:right;
    }
    .app-homeContainer .mui-table-view .mui-media-object {
    line-height: 80px;
    max-width:80px;
    height: 80px;
}
.app-homeContainer .mui-table-view li .mui-pull-left {
    width: 84px;
}
</style>
