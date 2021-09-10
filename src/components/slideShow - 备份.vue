<template>
  <div class="slide-box clearfix">
    <div class="slide-show clearfix" @mouseover="clearInv" @mouseout="runInv">
      <div class="slide-img">
        <a :href="slides[nowIndex].clickUrl">
          <transition name="slide-trans" enter-active-class="animated tada"
            leave-active-class="animated bounceOutRight">
            <img v-if="isShow" class="bigimg" :src="slides[nowIndex].src">
          </transition>
          <!-- <transition name="slide-trans-old">
						<img v-if="!isShow" :src="slides[nowIndex].src">
					</transition> -->
        </a>
      </div>
      <div class="slititle title ">{{ slides[nowIndex].title }}</div>
      <div class="switch">
        <span class="prev" @click="goto(prevIndex)"> </span>
        <span class="next" @click="goto(nextIndex)"> </span>
      </div>
    </div>
    <span class="btnImgFullScreen proscreen_btn" @click="fullscreenbtn(nowIndex)"><img
        src="../assets/fullscreenwrite.png" class="fullscreen" width="32px" title="全屏显示" alt="全屏显示"></span>
    <div class="slide-pages">
      <div @click="gotoup(prevIndex)" class="black hand"> <img src="../assets/up.png" alt=""></div>
      <div class="ulbox" id="ulbox" ref="ulbox" style="overflow: hidden;">
        <ul class="slideul" @mousewheel="runover" id="ul" ref="slideul" v-model="ulboxheight" style="margin-top: 10px;"
          :style={transform:transform}>
          <!-- @click="toTop"-->
          <li v-for="(item, index) in slides" :key="index" @click="goto(index)" @mouseover="licheck(index)">
            <div class="imgbox" :class="{'active':index === nowIndex}">
              <img class="imgthumb" :src="item.src" :title="index">
              <!--:src="index"  -->
            </div>
          </li>
        </ul>
      </div>
      <div @click="gotolot(nextIndex)" class="black hand"> <img src="../assets/down.png" alt=""></div>
    </div>

    <!-- 全屏 -->
    <div class="container2" v-if="container2">
      <!-- main -->
      <div class="imgBox2">
        <img class="img2" :src="imgsrc" />
        <div class="imgboxtit">{{slides[nowIndex].title}}</div>
      </div>
      <div class="showImg2_btnLeft" @click="goto(fullprevIndex)"></div>
      <div class="showImg2_btnRight" @click="goto(fullnextIndex)"></div>
      <!-- <a href="javascript:;" class="chooseImg2_btnLeft aBtn"></a>
      <a href="javascript:;" class="chooseImg2_btnRight aBtn"></a> -->
      <div class="btnExitFullScreen" @click="exitFullscreen()"><img src="../assets/smallscreen.png" width="32px"
          title="退出全屏" alt="退出全屏"></div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      slides: {
        type: Array,
        default: []
      },
      inv: {
        type: Number,
        default: 500000
      },
      height: {
        default: 111,
        type: Number
      },
      lineNum: {
        default: 5,
        type: Number
      }
    },
    data() {
      return {
        nowIndex: 0,
        isShow: true,
        scrollTop: 0,
        ulboxheight: 0,
        gotop: false,
        num: 0,
        imgsrc: '',
        container2: false,
        //imgBox2:
      }
    },
    computed: {
      transform: function() {
        return 'translateY(-' + this.num * this.height + 'px)'
      },
      prevIndex() {
        if (this.nowIndex === 0) {
          return 0
        } else {
          return this.nowIndex - 1
        }
      },
      nextIndex() {
        if (this.nowIndex === this.slides.length - 1) {
          return this.slides.length
        } else {
          return this.nowIndex + 1
        }
      },
      fullprevIndex() {
        var _this = this;
        let tempindexpre;
        if (_this.nowIndex === 0) {
          tempindexpre = _this.slides.length - 1;
        } else {
          tempindexpre = _this.nowIndex - 1
        }
        _this.setImg(tempindexpre);
        return tempindexpre
      },
      fullnextIndex() {
        let tempindex;
        if (this.nowIndex === this.slides.length - 1) {
          tempindex = 0;
        } else {
          tempindex = this.nowIndex + 1;
        }
        this.setImg(tempindex);
        return tempindex
      }
    },
    created: function() {
      let _this = this
      /* setInterval(function () {
        if (_this.num !== _this.slides.length) {
         // console.log(111);
          _this.num++
        } else {
         // console.log(222);
          _this.num = 0
        }
      }, 3000) */
      /* console.log(12)

       this.$nextTick(() => {
       console.log(this.$refs.img);
       }) */
    },
    mounted() {
      this.runInv();
      window.onresize = () => {
        if (!this.checkFull()) {
          // 要执行的动作
          this.container2 = false;
          this.exitFullscreen();
        }
      }
    },
    methods: {
      //全屏预览按钮
      fullscreenbtn(index) {
        this.setImg(index);
        this.enterFullscreen();
      },
      goto(index) {
        // console.log(index)
        if (index == this.slides.length) {
          return false;
        }
        //this.isShow = false
        setTimeout(() => {
          this.isShow = true
          this.nowIndex = index
          this.num = index
          if (index <= 2) {
            this.num = 0
          } else if (index >= (this.slides.length - 2)) {
            this.num = this.slides.length - 3
          } else {
            this.num = index - 2
          }
        }, 50)
      },
      gotoup() {
        var show_num = Math.floor(this.$refs.ulbox.offsetHeight / 111);
        //ul总个数
        var ultotal = this.slides.length;
        //总高再多加1/4个
        var cdtotal = ultotal + Math.floor(ultotal / 4);
        if (this.num < show_num) {
          if (this.num <= 4) {
            this.num = 0
          }
        } else {
          this.num -= show_num
        }
      },
      gotolot() {
        var show_num = Math.floor(this.$refs.ulbox.offsetHeight / 111);
        //ul总个数
        var ultotal = this.slides.length;
        //总高再多加1/4个
        var cdtotal = ultotal + Math.floor(ultotal / 4);
        if (this.num <= (ultotal - this.num)) {
          //console.log(1111)
          this.num += show_num
          //console.log("aaa",ultotal-this.num)
        } else {
          // 11                 5
          //console.log((ultotal-this.num));
          if ((ultotal - this.num) > show_num) {
            //console.log(222)
            this.num += show_num
          } else {
            //console.log(333)
            this.num = this.num
          }
        }
        //console.log(this.num)
        //console.log(ultotal-this.num)
        /* console.log(this.num<(ultotal-show_num))
         console.log('yu',ultotal%show_num)
         console.log("num:",this.num) */

      },
      runInv() {
        this.invId = setInterval(() => {
          this.goto(this.nextIndex)
        }, this.inv)
      },
      clearInv() {
        clearInterval(this.invId)
      },
      runover(e) {

      console.log(e.deltaY);

      var deltaY = e.deltaY;

var show_num = Math.floor(this.$refs.ulbox.offsetHeight / 111);

      if(deltaY > 0){

        // up
        if (this.num <= this.slides.length-show_num) {
          this.num++;
        }

      }else{
        //down

        if (this.num > 0){
          this.num--;
        }

      }




        /*    var show_num = Math.floor(this.$refs.ulbox.offsetHeight/111);
        //ul总个数
        var ultotal = this.slides.length;
        //总高再多加1/4个
        var cdtotal = ultotal + Math.floor(ultotal / 4);
 */

        //this.num -= this.num
     /*   console.log(11)
        if (this.num <= this.slides.length-show_num) {
          this.num++;
        } else {
          this.num--;
        }
 */
        //document.getElementById('ul').scrollTop -= 50



      },
      licheck(index) {
        /* this.isShow = false
        if(this.nowIndex  != index){
        	setTimeout(() => {
        		this.isShow = true
        		this.nowIndex = index
        	}, 50)
        } */
      },
      //设置全屏的图片
      setImg(index) {
        this.container2 = true;
        let src = this.slides[index]
        let urlsrc = src.src;
        this.imgsrc = urlsrc
        let img = new Image();
        img.src = urlsrc;
        img.width = '100%';
        img.height = '100%';
      },
      // 判断各种浏览器
      enterFullscreen() {
        var element = document.documentElement;
        if (element.requestFullscreen) {
          element.requestFullscreen();
        } else if (element.mozRequestFullScreen) {
          element.mozRequestFullScreen();
        } else if (element.webkitRequestFullscreen) {
          element.webkitRequestFullscreen();
        } else if (element.msRequestFullscreen) {
          element.msRequestFullscreen();
        }
      },
      checkFull() {
        var isFull =
          document.fullscreenElement ||
          document.mozFullScreenElement ||
          document.webkitFullscreenElement ||
          document.msFullscreenElement
        if (isFull === undefined) isFull = false
        return isFull
      },
      // 判断浏览器种类
      exitFullscreen() {
        if (document.exitFullScreen) {
          document.exitFullscreen()
        }
        //兼容火狐
        if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen()
        }
        //兼容谷歌等
        if (document.webkitExitFullscreen) {
          document.webkitExitFullscreen()
        }
        //兼容IE
        if (document.msExitFullscreen) {
          document.msExitFullscreen()
        }
      }
    }
  }
</script>

<style scoped>
  .clearfix:after {
    content: ".";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
    font-size: 0;
  }

  .clearfix {
    zoom: 1;
  }

  ul,
  li {
    margin: 0;
    padding: 0;
  }

  .black {
    display: block;
    width: 100%;
    text-align: center;
  }

  .hand {
    cursor: pointer;
  }

  .slide-box {
    /* max-width: 970px;
     max-width: 800px;
    margin: 0 auto;*/
    position: relative;
    background: #000;
  }

  .slideul {
    transition: all .3s ease;
  }

  .slide-trans-enter-active {
    transition: all 0.25s;
    transform: translateX(610px);
  }

  .slide-trans-enter {
    /* transform: translateX(610px); */
  }

  .slide-trans-old-leave-active {
    transition: all 0.25s;
    transform: translateX(-610px);

    /* transform: translateX(-610px); */
  }

  .slide-show {
    position: relative;
    /* margin: 0 auto;
		max-width: 610px;*/
    height: 570px;
    text-align: center;
    /* overflow: hidden; */
    background: #000;
    width: calc(100% - 160px);
  }

  .slide-show .slititle {
    position: absolute;
    width: 100%;
    color: #fff;
    background: rgba(0, 0, 0, .8);
    opacity: .5;
    bottom: 0;
    margin: 0;
    height: 3rem;
    line-height: 3rem;
  }

  .slide-img {

    overflow: hidden;
    text-align: center;
    display: table;
    float: left;
    position: relative;
    width: 100%;
    height: 100%;

  }

  .slide-img a {
    display: table-cell;
    vertical-align: middle;
    width: 100%;
    height: 100%;
    /* background-color: #0000FF; */
  }

  .slide-img img {
    max-width: 100%;
    max-height: 100%;
    margin: 0 auto;
    /*
		position: absolute;
		top: 0; */
  }

  .slide-pages {
    position: absolute;
    width: 160px;
    right: 0;
    top: 0;
    /* 		height: 100%;
		right: -60px;
		top: 285px;
		transform: translate(-50%, -50%); */
  }

  .slide-pages li {
    padding: 0 0.625rem;
    cursor: pointer;
    color: #fff;
    font-size: 1rem;
    width: 140px;
    height: 100px;
    display: table;
    text-align: center;
    display: table;
    float: left;
    position: relative;
    margin-bottom: 5px;
  }

  .imgbox {
    /* display: table-cell;
     max-height: 100%;*/
    max-width: 100%;
    height: 100px;

    vertical-align: middle;
    border: 3px solid #ccc;
  }

  .ulbox {
    height: 512px;
    overflow-x: hidden;
    overflow-y: scroll;
  }

  .slide-pages li .on {
    color: deeppink;
    text-decoration: underline;
  }

  .imgthumb {
    max-width: 134px;
    height: 100px;
  }

  .bigimg {
    transition: all 1s ease 1s;
  }

  .title {
    text-align: center;
  }

  .switch {}

  .switch span {
    position: absolute;
    width: 82px;
    height: 82px;
    /* text-align: center;
    background-color: rgba(0, 0, 0, .3);
    font-size: 1.25rem;
    color: #ffffff;*/
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
    font-family: "宋体";
  }

  /* .switch span:hover {
    background-color: rgba(0, 0, 0, .7);
  } */

  .prev {
    left: 10px;
    background: url(../assets/prev.png);
  }

  .prev:hover {
    background: url(../assets/prev2.png);
  }

  .next {
    right: 0;
    background: url(../assets/next.png);
  }

  .next:hover {
    background: url(../assets/next2.png);
  }

  .active {
    border: 3px solid red;
  }

  /* 设置滚动条的样式 */
  ::-webkit-scrollbar {
    width: 1px;
  }

  /* 滚动槽 */
  ::-webkit-scrollbar-track {
    -webkit-box-shadow: inset006pxrgba(0, 0, 0, 0.3);
    border-radius: 1px;
  }

  /* 滚动条滑块 */
  ::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background: rgba(0, 0, 0, 0.1);
    -webkit-box-shadow: inset006pxrgba(0, 0, 0, 0.5);
  }

  ::-webkit-scrollbar-thumb:window-inactive {
    background: #333;
  }

  .proscreen_btn {
    position: absolute;
    top: 10px;
    right: 160px;
    cursor: pointer;
  }

  /* 全屏 */
  .container2 {
    width: 100%;
    height: 100%;
    background-color: #262626;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1001;
    /* display: none; */
  }

  .btnExitFullScreen {
    color: #989898;
    position: absolute;
    top: 1%;
    right: 1%;
    font-size: 20px;
    line-height: 20px;
    cursor: pointer;
  }

  .chooseTimeBox {
    position: absolute;
    width: 70px;
    text-align: center;
    height: 20px;
    line-height: 20px;
    background-color: #3d3d3d;
    left: calc(50% - 35px);
    top: 1.5%;
  }

  .chooseTimeBox * {
    color: #e1e1e1;
  }

  .chooseTimeBox .select {
    background: #121212;
    color: #999999;
    width: 40px;
    height: 18px;
    left: 2px;
    top: 1px;
    outline: none;
    display: none;
    float: left;
    position: relative;
    top: 1px;
  }

  .chooseTimeBox .btnStart {
    -display: none;
  }

  .chooseTimeBox .btnStop {
    display: none;
    position: relative;
    top: -1px;
  }

  .imgBox2 {
    position: absolute;
    width: 70%;
    height: calc(90% - 144px);
    -border: 1px solid red;
    left: 15%;
    top: 8%;
  }

  .imgBox2 img {
    position: absolute;
  }

  .imgboxtit {
    width: 100%;
    font-size: 24px;
    height: 70px;
    line-height: 70px;
    text-align: center;
    color: #fff;
    position: absolute;
    left: 0;
    bottom: 0;
  }

  .showImg2_btnLeft,
  .showImg2_btnRight {
    width: 50%;
    height: calc(90% - 144px);
    position: absolute;
    top: 8%;
    -border: 1px solid #fff;
  }

  .showImg2_btnLeft {
    left: 0;
    cursor: url(https://static.mfcad.com/2021/images/cur_left.png), auto;
  }

  .showImg2_btnRight {
    right: 0;
    cursor: url(https://static.mfcad.com/2021/images/cur_right.png), auto;
  }

  .showImg2_btnLeft {
    left: 0;
    cursor: url(https://static.mfcad.com/2021/images/cur_left.ico), auto;
  }

  .showImg2_btnRight {
    right: 0;
    cursor: url(https://static.mfcad.com/2021/images/cur_right.ico), auto;
  }

  .imgListBox2 {
    position: absolute;
    width: 80%;
    height: 140px;
    border: 1px solid #3e3e3e;
    left: 10%;
    bottom: 2%;
    overflow: hidden;
  }

  .imgList2 {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 110px;
    top: 15px;
    transition: left 0.5s;
  }

  .imgList2 li {
    width: 110px;
    height: 110px;
    box-sizing: border-box;
    border: 1px solid #707070;
    float: left;
    margin-right: 5px;
    /* background-image: url(../images/a2.jpg); */
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    cursor: pointer;
  }

  .imgList2 li.active {
    border: 2px solid #2f95d5;
  }

  .container2 .aBtn {
    position: absolute;
    display: block;
    width: 25px;
    height: 70px;
    margin: 15px 2px 0;
  }

  .container2 .aBtn:hover {
    background-color: #e6e6e6;
  }

  .container2 .aBtn.disable {
    background-color: #fff;
  }

  /* .container2 .aBtn:before{
    content: ''; display: block; position: absolute; width: 14px; height: 27px; left: calc(50% - 7px); top: calc(50% - 13px); background: url(../images/btn.png);
  } */
  .container2 .chooseImg2_btnLeft {
    left: calc(10% - 50px);
    bottom: calc(2% + 35px);
  }

  .container2 .chooseImg2_btnRight {
    right: calc(10% - 50px);
    bottom: calc(2% + 35px);
  }

  .container2 .chooseImg2_btnLeft:before,
  .container2 .chooseImg2_btnLeft.disable:hover:before {
    background-position: -27px -174px;
  }

  .container2 .chooseImg2_btnLeft.disable:hover:before {
    cursor: default;
  }

  .container2 .chooseImg2_btnLeft:hover:before {
    background-position: -73px -174px;
  }

  .container2 .chooseImg2_btnRight:before,
  .container2 .chooseImg2_btnRight.disable:hover:before {
    background-position: 0 -174px;
  }

  .container2 .chooseImg2_btnRight.disable:hover:before {
    cursor: default;
  }

  .container2 .chooseImg2_btnRight:hover:before {
    background-position: -46px -174px;
  }

  .img2 {
    display: block;
    object-fit: scale-down;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  /*  .bobtn{
  	position: absolute; z-index:3;left: 50%; top:50%; margin-top: -64px !important;margin-left: -64px !important; height: 128px !important; cursor: pointer;
  	background: url(../images/bo.png);
  	width: 128px;
  }
  .bosmallbtn{
  	position: absolute; z-index:3;left: 50%; top:50%; margin-top: -16px;margin-left: -16px; height: 32px !important; cursor: pointer;
  	background: url(../images/bosmall.png);
  	width: 32px;
  } */
</style>
