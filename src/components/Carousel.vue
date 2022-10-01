<template>
  <div id="carousel">
    <split-carousel
      v-bind:speed="getSpeedMs"
      v-bind:interval="getIntervalMs"
      v-bind:display-amount="displayItems"
      v-bind:item-width="itemWidth"
      v-bind:height="itemHeight"
    >
      <split-carousel-item v-for="(eachImage, index) in images" v-bind:key="index">
        <div class='item'>
          <img v-bind:src='eachImage' alt="Image Not Found">
        </div>
      </split-carousel-item>
    </split-carousel>

    <label for="speed">Speed:</label>
    <input id="speed" type="text" v-model="speedMs">
    <br>
    <label for="interval">Interval:</label>
    <input id="interval" type="text" v-model="intervalMs">
    <br>
  </div>
</template>

<script>
  // See more 
  // DEMO: https://aaron00101010.github.io/vue-split-carousel/
  import { SplitCarousel, SplitCarouselItem } from "vue-split-carousel";

  export default {
    name: 'image-carousel',
    components: {
      SplitCarousel,
      SplitCarouselItem
    },
    data(){
      return{
        speedMs: 500,
        intervalMs: 2000,
        displayItems: 2,
        itemWidth: 200,
        itemHeight: 250,
        arrowChecked: true,
        images: [],
      }
    },
    computed:{
      getIntervalMs(){return Number.parseInt(this.intervalMs)},
      getSpeedMs(){return Number.parseInt(this.speedMs)},
    }, 
    methods: {
      loadImages(context){
        context.keys().forEach( imageFileName => {
          // Why require() is needed
          // https://stackoverflow.com/questions/71490048/img-src-with-v-bind-not-displaying-image
          this.images.push(require(`@/assets/images/${imageFileName.slice(2)}`))
        })
      }
    },
    mounted() {
      /*
       * https://webpack.js.org/guides/dependency-management/
       * require.context(<directory>, <search-sub-directories>, <regex-file-match>, <mode>)
       */
      this.loadImages(require.context('@/assets/images/', true, /\.(png|jp?g|png)$/));
    },
  };
</script>

<style scoped>
  #carousel {
    width: 600px;
    height: 300px;
    margin: 0 auto;
  }
</style>