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

export default {
  data () {
    return {
      imgsArrangeArr: []
    }
  },
  mounted(){
  	this.imgsArrangeArr = (function(arr){
  		for(var index = 0 , imgItem ; imgItem = arr[index++];){
  			imgItem.imgUrl = require('../images/' + imgItem.imgName);
  		}
  		return arr;
  	})(imgsData);
  },
  methods : {
  	getRangeRandom(low , high){
  		return Math.floor(Math.random()*(high - low) + low);
  	} ,
  	get30DegRandom(){
  		return ((Math.random() > 0.5 ? '+' : '-') + Math.ceil(Math.random() * 30));
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

