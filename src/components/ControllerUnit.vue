<template>
	<span class="controller-unit"	v-on:click="handleClick"
		v-bind:class="{'is-inverse' : isInverse , 'is-center' : isCenter}">
	</span>
</template>

<script>
export default {
  data () {
    return {
      isCenter : false ,
      isInverse : false 
    }
  } ,
  watch: {
    arrange :{
        handler(val , oldVal){
           this.handleRange();
        } ,
        deep : true
    }
  } ,
  props : {
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
	this.handleRange(); 
  } ,
  methods :{
  	handleRange(){		//排列
	    this.isCenter = this.arrange.isCenter;
	    this.isInverse = this.arrange.isInverse;
	 } ,
	 handleClick(e){			//处理点击事件
	 	if(this.arrange.isCenter){
           this.$emit('inverse');
           this.isInverse = this.arrange.isInverse;
        }else{
           this.$emit('center');
           this.isCenter = this.arrange.isCenter;
        }
        e.stopPropagation();
        e.preventDefault();
	 }
  }
}
</script>

