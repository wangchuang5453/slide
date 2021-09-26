<template>
  <div class="p-index main" id="con"> 
    <slide-item>
      <div class="bg"></div>
    </slide-item>
    <slide-item>
      <div class="bg1"></div>
    </slide-item>
    <slide-item>
      <div class="bg2"></div>
    </slide-item>
    <slide-item>
      <div class="bg3"></div>
    </slide-item>
    <!-- <section class='u-arrow'><img src="image2/btn01_arrow.png" /></section> -->
</div>

</template>

<script>
import slideItem from './slide-item.vue';
export default {
  name: 'HelloWorld',
  mounted() {
    this.initPage();
  },
  provide() {
    return {
      slide: this
    }
  },
  data() {
    return {
      page_n: 1, //初始页面位置
      indexP: null, //控制首页不能直接找转到最后一页
      newM: null, //重新加载的浮层
    }
  },
  components: {
    slideItem,
  },
  methods: {
    toSetIndexP(value) {
      this.indexP = value;
    },
    setActive(top) {
      this.$children[this.newM - 1].setActive(top);
    },
    getNewMTop() {
      console.log(this.$children[this.newM - 1].top);
      return Number(this.$children[this.newM - 1].top.split('px')[0]);
    },
    setItem(index, translateYValue, scaleValue) {
      this.$children[index].setItem(translateYValue, scaleValue);
    },
    setToTop(index) {
      this.$children[index].setToTop();
    },
    setToTopBack(index) {
      this.$children[index].setToTopBack();
    },
    setToBottomBack(index) {
      this.$children[index].setToBottomBack();
    },
    initPage() {
      this.$children[this.page_n - 1].initPage();
    },
    setRestart() {
      this.$children[this.page_n - 1].setRestart();
    },
    resetPage() {
      this.$children[this.page_n - 1].delActiveStatus();
      this.$children[this.page_n - 1].resetShowStatus(false);
      this.$children[this.newM - 1].delActiveStatus();
      this.$children[this.newM - 1].resetShowStatus(true);
    },
    resetPageBack() {
      // console.log(this.page_n);
      this.$children[this.page_n - 1].delActiveStatusBack();
      this.$children[this.newM - 1].delActiveStatusBack();
      this.$children[this.newM - 1].resetShowStatus(false);
    },
    resetToBottomPageBack() {
      this.$children[this.page_n - 1].delActiveStatusBottomBack();
      this.$children[this.newM - 1].delActiveStatusBottomBack();
      this.$children[this.newM - 1].resetShowStatus(false);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
// img{user-select:​ none;}
.main{
  width:100%;height:100%;
  margin:0 auto;
  box-sizing: border-box;
  position:relative;
  overflow: hidden;
}

.u-arrow {
  position: fixed;
  bottom: 10px; left:50%;
  z-index: 150;
  width: 24px; height: 14px;
  margin-left: -7px;
}
.u-arrow img {
  position: absolute;
  top: 50%; left: 50%;
  margin: -7px 0 0 -14px;
  width: 24px; height: 14px;
  background-position: 0 -82px;
	animation: start 1.5s infinite ease-in-out;
}
.bg {
  background-color: red;
  width: 100%;
  height: 100%;
}
.bg1 {
  background-color: blue;
  width: 100%;
  height: 100%;
}
.bg2 {
  background-color: yellow;
  width: 100%;
  height: 100%;
}
.bg3 {
  background-color: black;
  width: 100%;
  height: 100%;
}
</style>
