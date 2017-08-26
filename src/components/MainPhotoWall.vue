<template>
	<section class="stage" id="stage" ref="stage" >
        <section class="img-sec" ref="imgSec">
            <image-figure   v-for="(item , index) in imgsObj" :key="item.id"  :data="item" 
            		:arrange="imgsArrangeArr[index]" v-on:init="start" v-on:inverse="inverse(index)" v-on:center="center(index)" >
            	
            </image-figure>
        </section>
        <nav class="controller-nav" >
           	<controller-unit v-for="(item , index) in imgsObj" :key="item.id" 
           		:arrange="imgsArrangeArr[index]"  v-on:inverse="inverse(index)" v-on:center="center(index)" >
           		
           	</controller-unit>
        </nav>
        <canvas-effect ></canvas-effect >
    </section>
</template>

<script>
require('../lib/zepto.min.js');
	
import imgsData from '../data/imageDatas.json';
import imageFigure from './imageFigure.vue';
import controllerUnit from './controllerUnit.vue';
import canvasEffect from './CanvasEffect.vue';


export default {
  data () {
    return {
      imgsArrangeArr: [] ,
      imgsObj : [] ,
      Constant : {       	//定义界面图片的显示范围的坐标
      		isInit : false ,
            centerPos: {   	 //中心位置
                left : 0 ,
                right : 0
            } ,
            hPosRange: {      	//水平方向的取值范围，分为左边的部分和右边的部分
                leftSecX : [0 , 0] ,
                rightSecX : [0 , 0] ,
                y : [0 , 0]
            } ,
            vPosRange: {      	//垂直方向的取值范围
                x : [0 , 0],
                topY : [0 , 0]
            }
        }
    }
  } ,
  mounted () {
  	var _this = this;
  	this.imgsObj = (function(arr){
  		for(var index = 0 , imgItem ; imgItem = arr[index++];){
  			imgItem.imgUrl = require('../images/' + imgItem.imgName);
  			_this.imgsArrangeArr.push({});
  		}
  		return arr;
  	})(imgsData);
  } ,
  methods : {
  	getRangeRandom(low , high){
  		return Math.floor(Math.random()*(high - low) + low);
  	} ,
  	get30DegRandom(){
  		return ((Math.random() > 0.5 ? '+' : '-') + Math.ceil(Math.random() * 30));
  	} ,
  	rearrange(index){			//处理中心图片
  		 let imgsArrangeArr = this.imgsArrangeArr , Constant = this.Constant ,
            centerPos = Constant.centerPos , hPosRange = Constant.hPosRange ,
            vPosRange = Constant.vPosRange;
        //第一步：处理位于中心的图片
        let centerImgArr = imgsArrangeArr.splice(index , 1);
        centerImgArr[0] = {
            pos : centerPos ,
            rotate : 0 ,
            isCenter : true
        }
        //第二步：处理中心轴的图片
        let topImgNum = Math.floor(Math.random() * 2);    //即1或0
        let topImgIndex = Math.floor(Math.random() * (imgsArrangeArr.length - topImgNum));
        let topImgArr = imgsArrangeArr.splice(topImgIndex , topImgNum);
        topImgArr.forEach( (val , index) => {
            topImgArr[index] = {
                pos : {
                    top : this.getRangeRandom(vPosRange.topY[0] , vPosRange.topY[1]) ,
                    left : this.getRangeRandom(vPosRange.x[0] , vPosRange.x[1])
                } ,
                rotate : this.get30DegRandom() ,
                isCenter : false
            }
        });
        //第三步：处理左右两侧的图片
        for(let i = 0 , j = imgsArrangeArr.length , k = j/2 ; i < j ; i++){
            let xPosRange = i < k ? hPosRange.leftSecX : hPosRange.rightSecX;
            imgsArrangeArr[i] = {
                pos : {
                    top : this.getRangeRandom(hPosRange.y[0] , hPosRange.y[1]) ,
                    left : this.getRangeRandom(xPosRange[0] , xPosRange[1])
                } ,
                rotate : this.get30DegRandom() ,
                isCenter : false
            }
        }
        //第四步：拼接数组，并重新设置到state中
        if(topImgArr && topImgArr[0]) imgsArrangeArr.splice(topImgIndex , 0 , topImgArr[0]);
        imgsArrangeArr.splice(index , 0 , centerImgArr[0]);
  	} ,
  	initConstant(){				//初始化Constant中的数据
  		this.Constant.isInit = true;
  		//获取整个界面的高宽，以及相关参数
        let stageDOM = $("#stage")[0] ,
            stageW = stageDOM.scrollWidth , stageH = stageDOM.scrollHeight ,
            halfStageW = Math.ceil(stageW/2) , halfStageH = Math.ceil(stageH/2);
        //获取imgFigure的高宽，以及相关参数
        let imgFigureDOM = $(".img-figure").eq(0)[0] ,
            imgW = imgFigureDOM.scrollWidth , imgH = imgFigureDOM.scrollHeight ,
            halfImgW = Math.ceil(imgW/2) , halfImgH = Math.ceil(imgH/2);
        //中心图片的位置
        this.Constant.centerPos = {
            left : halfStageW - halfImgW ,
            top : halfStageH - halfImgH
        }
        //左右两边图片位置的取值范围
        this.Constant.hPosRange.leftSecX[0] = -halfImgW;
        this.Constant.hPosRange.leftSecX[1] = halfStageW - 3 * halfImgW;
        this.Constant.hPosRange.rightSecX[0] = halfStageW + halfImgW;
        this.Constant.hPosRange.rightSecX[1] = stageW - halfImgW;
        this.Constant.hPosRange.y[0] = -halfImgH;
        this.Constant.hPosRange.y[1] = stageH - halfImgH;
        //中轴线图片位置取值范围
        this.Constant.vPosRange.x[0] = halfStageW - imgW;
        this.Constant.vPosRange.x[1] = halfStageW;
        this.Constant.vPosRange.topY[0] = -halfImgH;
        this.Constant.vPosRange.topY[0] = halfStageH - 3 * halfImgH;
        
  	} ,
  	start(){				//页面中的图片开始排序
  		if(this.Constant.isInit) return;
  		//第一步 初始化数据
  		this.initConstant();
  		//第二步 排位置
  		var centerIndex = Math.floor(Math.random() * this.imgsObj.length);
  		this.rearrange(centerIndex);
  	} ,
  	inverse(index){      /*图片翻转*/
        this.$set(this.imgsArrangeArr[index] , "isInverse" , !this.imgsArrangeArr[index].isInverse);
    } ,
    center(index){      /*图片居中*/
        this.rearrange(index);
    }
  } ,
  components: {
	imageFigure , controllerUnit , canvasEffect
  }
}
</script>


<style lang="scss">
	@import '../styles/App.scss';
</style>

