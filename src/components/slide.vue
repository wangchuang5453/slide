<template>
  <div class="p-index main" id="con"> 
    <!-- <section class='u-arrow'><img src="image2/btn01_arrow.png" /></section> -->
    <slot></slot>
</div>

</template>

<script>
import slideItem from './slide-item.vue';
export default {
  name: 'HelloWorld',
  mounted() {
    this.initPage();
  },
  props: {
    fastMode: {
      type: Boolean,
      default: true,
    },
    maxSize: {
      type: Number,
      require: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    disabledToNextPage: {
      type: Boolean,
      default: false,
    },
    disabledToLastPage: {
      type: Boolean,
      default: false,
    },
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
    getChildById(index) {
      const slots = this.$slots.default;
      const slideName = slots[index].child.slideName;
      const instance = this.$children.find((item) => item.slideName == slideName);
      return instance;
    },
    toSetIndexP(value) {
      this.indexP = value;
    },
    setActive(top) {
      // this.$children[this.newM - 1].setActive(top);
      this.getChildById(this.newM - 1).setActive(top);
    },
    getNewMTop() {
      // return Number(this.$children[this.newM - 1].top.split('px')[0]);
      return Number(this.getChildById(this.newM - 1).top.split('px')[0]);
    },
    setItem(index, translateYValue, scaleValue) {
      // this.$children[index].setItem(translateYValue, scaleValue);
      this.getChildById(index).setItem(translateYValue, scaleValue);
    },
    setToTop(index, direction, slideName) {
      // this.$children[index].setToTop();
      this.getChildById(index).setToTop();
      this.emitPageChange(index, direction, slideName);
    },
    setToTopBack(index) {
      // this.$children[index].setToTopBack();
      this.getChildById(index).setToTopBack();
    },
    setToBottomBack(index) {
      // this.$children[index].setToBottomBack();
      this.getChildById(index).setToBottomBack();
    },
    initPage() {
      // this.$children[this.page_n - 1].initPage();
      const slideName = this.getChildById(this.page_n - 1).initPage();
      this.emitPageChange(this.page_n - 1, -1, '');
      this.emitPageChangeEnd(slideName);
    },
    setRestart() {
      // this.$children[this.page_n - 1].setRestart();
      this.getChildById(this.page_n - 1).setRestart();
    },
    resetPage() {
      this.getChildById(this.page_n - 1).delActiveStatus();
      this.getChildById(this.page_n - 1).resetShowStatus(false);
      this.getChildById(this.newM - 1).delActiveStatus();
      this.getChildById(this.newM - 1).resetShowStatus(true);
      // this.$children[this.page_n - 1].delActiveStatus();
      // this.$children[this.page_n - 1].resetShowStatus(false);
      // this.$children[this.newM - 1].delActiveStatus();
      // this.$children[this.newM - 1].resetShowStatus(true);
    },
    resetPageBack() {
      // this.$children[this.page_n - 1].delActiveStatusBack();
      // this.$children[this.newM - 1].delActiveStatusBack();
      // this.$children[this.newM - 1].resetShowStatus(false);
      this.getChildById(this.page_n - 1).delActiveStatusBack();
      this.getChildById(this.newM - 1).delActiveStatusBack();
      this.getChildById(this.newM - 1).resetShowStatus(false);
    },
    resetToBottomPageBack() {
      // this.$children[this.page_n - 1].delActiveStatusBottomBack();
      // this.$children[this.newM - 1].delActiveStatusBottomBack();
      // this.$children[this.newM - 1].resetShowStatus(false);
      this.getChildById(this.page_n - 1).delActiveStatusBottomBack();
      this.getChildById(this.newM - 1).delActiveStatusBottomBack();
      this.getChildById(this.newM - 1).resetShowStatus(false);
    },
    emitPageChange(page, direction, slideName) {
      this.$emit('pageChange', {page, direction, lastSlideName: slideName});
    },
    goToNextPageDirectly() {
      // this.$children[this.page_n - 1].goToNextPageDirectly();
      this.getChildById(this.page_n - 1).goToNextPageDirectly();
    },
    emitPageChangeEnd(activeSlideName) {
      this.$emit('pageChangeEnd', {activeSlideName});
    },
  }
}
</script>
<style scoped lang="less">
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
</style>
