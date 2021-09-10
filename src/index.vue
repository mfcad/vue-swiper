<template>
  <div>
   <!-- <div style="height: 1000px;">safdasfsadf</div> -->

    <div class="index-wrap">
      <slide-show :slides="slides"></slide-show>
    </div>
  </div>

</template>

<script>
import slideShow from './components/slideShow'
export default {
  components: {
    slideShow
  },
  data () {
    return {
      slides: [
        {
          src: require('./assets/1.jpg'),
          title: '图片1',
          clickUrl:'#'
        },
        {
          src: require('./assets/2.jpg'),
          title: '图片2',
          clickUrl:'#'
        },
        {
          src: require('./assets/3.jpg'),
          title: '图片3',
          clickUrl:'#'
        },
        {
          src: require('./assets/4.jpg'),
          title: '图片4',
          clickUrl:'#'
        },
        {
          src: require('./assets/3.jpg'),
          title: '图片3',
          clickUrl:'#'
        },
        {
          src: require('./assets/4.jpg'),
          title: '图片4',
          clickUrl:'#'
        },
        {
          src: require('./assets/3.jpg'),
          title: '图片3',
          clickUrl:'#'
        },
        {
          src: require('./assets/4.jpg'),
          title: '图片4',
          clickUrl:'#'
        },
       /* {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        },
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        }, */
        {
          src: require('./assets/5.jpg'),
          title: '图片5',
          clickUrl:'#'
        }
      ]
    }
  },
  methods:{
  }
}
</script>

<style scoped>
  .index-wrap {
   /* max-width: 970px; */
    margin: 0 auto;
    /* overflow: hidden; */
}

</style>
