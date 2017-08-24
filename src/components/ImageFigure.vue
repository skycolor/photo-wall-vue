<template>
	<figure class="img-figure">
        <img  :src="imgUrl" />
        <figcaption c>
            <h2 class="img-title" >{{imgTitle}}</h2>
            <div class="img-back"  >
                {{imgDesc}}
            </div>
        </figcaption>
    </figure>
</template>

<script>
export default {
  data () {
    return {
      imgUrl : this.arrange.imgUrl ,
      imgTitle : this.arrange.title ,
      imgDesc : this.arrange.desc
    }
  },
  props : {
  	arrange : Object
  } ,
  mounted(){
  	
  }
}
</script>

