<template>
	<section class="stage" ref="stage" >
        <section class="img-sec">
            <image-figure v-for="item in imgsArrangeArr" :key="item.id"  :arrange="item" >
            	
            </image-figure>
        </section>
        <nav class="controller-nav" >
           	<controller-unit v-for="item in imgsArrangeArr" :key="item.id"  :arrange="item" >
           		
           	</controller-unit>
        </nav>
    </section>
</template>

<script>
import imgsData from '../data/imageDatas.json';
import imageFigure from './imageFigure.vue';
import controllerUnit from './controllerUnit.vue';
import $ from '../lib/zepto.min.js';

export default {
  data () {
    return {
      imgsArrangeArr: [] ,
      Constant : {       	//定义界面图片的显示范围的坐标
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
  	this.imgsArrangeArr = (function(arr){
  		for(var index = 0 , imgItem ; imgItem = arr[index++];){
  			imgItem.imgUrl = require('../images/' + imgItem.imgName);
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
                    top : getRangeRandom(vPosRange.topY[0] , vPosRange.topY[1]) ,
                    left : getRangeRandom(vPosRange.x[0] , vPosRange.x[1])
                } ,
                rotate : get30DegRandom() ,
                isCenter : false
            }
        });
        //第三步：处理左右两侧的图片
        for(let i = 0 , j = imgsArrangeArr.length , k = j/2 ; i < j ; i++){
            let xPosRange = i < k ? hPosRange.leftSecX : hPosRange.rightSecX;
            imgsArrangeArr[i] = {
                pos : {
                    top : getRangeRandom(hPosRange.y[0] , hPosRange.y[1]) ,
                    left : getRangeRandom(xPosRange[0] , xPosRange[1])
                } ,
                rotate : get30DegRandom() ,
                isCenter : false
            }
        }
        //第四步：拼接数组，并重新设置到state中
        if(topImgArr && topImgArr[0]) imgsArrangeArr.splice(topImgIndex , 0 , topImgArr[0]);
        imgsArrangeArr.splice(index , 0 , centerImgArr[0]);
        this.setState({
            imgsArrangeArr : imgsArrangeArr
        });
  	}
  } ,
  components: {
	imageFigure , controllerUnit
  }
}
</script>


<style lang="scss">
	@import '../styles/App.scss';
</style>

