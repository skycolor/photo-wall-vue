<template>
	<figure class="img-figure" v-bind:style="styleObj" v-on:click="handleClick"
		v-bind:class="{ 'is-inverse' : isInverse}"> 
        <img  :src="data.imgUrl" />
        <figcaption c>
            <h2 class="img-title" >{{data.title}}</h2>
            <div class="img-back"  >
                {{data.desc}}
            </div>
        </figcaption>
    </figure>
</template>

<script>
export default {
  data () {
    return {
      styleObj : {} ,
      isInverse : false
    }
  },
  watch: {
    arrange :{
        handler(val , oldVal){
           this.handlePos();
        } ,
        deep : true
    }
  },
  props : {
  	data : Object ,
  	arrange : {
      type: Object,
      default: function () {
	    return {
	      	pos : {
				top : 0 , left : 0
	        } ,
	        rotate : 0 ,
	        isInverse : false ,
	        isCenter : false
      	}
	  }
    }, 
  } ,
  mounted(){
  	this.$emit('init');
	this.handlePos(); 
  } ,
  methods :{
  	handlePos(){		//排列
  		let styleObj = {} , arrange = this.arrange;
  		if(arrange && arrange.pos) styleObj = (function(pos){
    		return {
    			top : pos.top + "px" , left : pos.left + "px"
    		};
	    })(arrange.pos);
	    //设置图片的旋转角度
	    if(arrange && arrange.rotate){
	        ['Moz', 'Ms', 'Webkit', ''].forEach((val) => {
	            styleObj[val + 'Transform'] = 'rotate(' + arrange.rotate + 'deg)';
	        });
	    }
	    //设置中心图片的z-index
	    styleObj.zIndex = (arrange && arrange.isCenter) ? 99 : 1;
	    this.styleObj = styleObj;
	    this.isInverse = arrange.isInverse;
	 } ,
	 handleClick(e){			//处理点击事件
	 	if(this.arrange.isCenter){
           this.$emit('inverse');
        }else{
           this.$emit('center');
        }
        e.stopPropagation();
        e.preventDefault();
	 }
  }
  
}
</script>

