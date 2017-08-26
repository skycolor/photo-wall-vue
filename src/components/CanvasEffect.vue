<template>
  	<div>
	    <canvas id="canvas1" class="snow-canvas"></canvas>
	    <canvas id="canvas2" class="snow-canvas"></canvas>
	    <canvas id="canvas3" class="snow-canvas"></canvas>
	</div>
</template>

<script>
require('../lib/zepto.min.js');
require('../lib/canvasLib');

export default {
  mounted(){
	this.handleCanvas1();
	this.handleCanvas2();
	this.handleCanvas3();
	$(".snow-canvas").snow();
  } ,
  methods : {
  	handleCanvas1(){
  		$("#canvas1").attr({
            speed : "1" ,
            interaction : "false" ,
            size : "5" ,
            count : "100" ,
            opacity : "0.00001" ,
            "wind-power" : "0" ,
            image : "false" ,
            width : "1272" ,
            height : "150"
        });
  	},
  	handleCanvas2(){
  		$("#canvas2").attr({
            speed : "3" ,
            interaction : "true" ,
            size : "10" ,
            count : "45" ,
            opacity : "0.00001" ,
            "wind-power" : "3" ,
            image : "false" ,
            width : "1272" ,
            height : "150"
        });
  	},
  	handleCanvas3(){
  		const snowImg = require('../images/snow.png');
  		$("#canvas3").attr({
            speed : "3" ,
            interaction : "true" ,
            size : "12" ,
            count : "35" ,
            "wind-power" : "-5" ,
            image : snowImg ,
            width : "1272" ,
            height : "150"
        });
  	}
  }
}
</script>

