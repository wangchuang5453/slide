<template>
  <section class="m-page"
    ref="mPage"
    :slideName="slideName"
    :style="{'top': top, 'transform': 'translate(0px,-' + translateY + 'px) scale('+ scale +')', }"
    :class="{'active': showActive, 'show': show, 'hide': !show, 'animationTop': animationTop, 'animationTopBack': animationTopBack, 'animationBottomBack': animationBottomBack}"
    @mousedown="touchstart"
    @touchstart="touchstart"
    @mousemove="touchmove"
    @touchmove="touchmove"
    @mouseup="touchend"
    @mouseout="touchend"
    @touchend="touchend"
  >
    <div class="m-img" >
      <slot></slot>
    </div>
  </section>
</template>
<script>
  export default {
    name: 'SlideItem',
    componentName: 'SlideItem',
    mounted() {
      this.v_h = document.documentElement.clientHeight + 'px';
      this.winHeight = document.documentElement.clientHeight;
      this.$el.addEventListener('animationend', this.animationend, false);
    },
    inject: ['slide'],
    props: {
      slideName: {
        type: String,
        require: true,
      },
    },
    data() {
      return {
        initP: null, //初值控制值
        mousedown: null, //鼠标按下的控制值
        firstP: null,	//第一次获取的值
        start: true, //控制动画开始
	      startM: null,	//开始移动
        moveP: null, //每次获取到的值
        p_b: null, //方向控制值
        position: null, //方向值
        canmove: false,		//首页返回最后一页
        move: null,	//触摸能滑动页面
        // Msize: 4,
        newM: null, //重新加载的浮层
        v_h: null,
        DNmove: false, //其他操作不让页面切换
        showActive: false,
        top: '0%',
        translateY: 0,
        scale: 1,
        winHeight: null, //= $(window).height(),
        show: false,
        animationTop: false,
        animationTopBack: false,
        animationBottomBack: false,
      }
    },
    methods: {
      touchstart(e) {
        if (this.slide.disabled) {
          return;
        }
        if (e.type == "touchstart") {
          this.initP = window.event.touches[0].pageY;
        } else {
          this.initP = e.y || e.pageY;
          this.mousedown = true;
        }
        this.firstP = this.initP;
      },
      /**
       * 触摸移动（鼠标移动）开始函数
       */
      touchmove(e) {
        if (this.slide.disabled || (this.slide.disabledToLastPage && this.slide.disabledToNextPage)) {
          return;
        }
        e.preventDefault();
		    e.stopPropagation();
        
        //判断是否开始或者在移动中获取值
        if(this.start || this.startM){
          this.startM = true;
          if (e.type == "touchmove") {
            this.moveP = window.event.touches[0].pageY;
          } else {
            if(this.mousedown) this.moveP = e.y || e.pageY;
          }
          if (this.slide.page_n == 1) {
            this.slide.indexP = false; //true 为不是第一页 false为第一页
          } else {
            this.slide.indexP = true;
          }
        }
        //设置一个页面开始移动
		    if(this.moveP && this.startM){ //&& imgs>1
			
			    //判断方向并让一个页面出现开始移动
          if(!this.p_b){
            this.p_b = true;
            this.position = this.moveP - this.initP > 0 ? true : false;	//true 为向下滑动 false 为向上滑动
            if(this.position && !this.slide.disabledToLastPage){
              //向下滑动
              if(this.slide.indexP){ // true为不是第一页								
                this.slide.newM = this.slide.page_n - 1 ;
                // 页面提前放置在距离屏幕下沿一屏高度的位置
                this.slide.setActive(`-${this.v_h}`);
                this.move = true;
              } else { // false为第一页
                this.move = false;
              }
            } 
            if (!this.position && !this.slide.disabledToNextPage) {
              //向上滑动
              if(this.slide.page_n != this.slide.maxSize){
                // if(!this.indexP) $('.audio_txt').addClass('close');
                this.slide.newM = this.slide.page_n + 1;
                // 页面提前放置在距离屏幕上沿一屏高度的位置
                this.slide.setActive(this.v_h);
                this.move = true;
              } else {
                this.move = false;
              }
            } 
			    }

          if (!this.slide.fastMode) {
            //根据移动设置页面的值
            if(!this.DNmove){
              //滑动带动页面滑动
              if(this.move){
                //移动中设置页面的值（top）
                this.start = false;
                
                const topV = parseInt(this.slide.getNewMTop());
                console.log(topV, '==== topv');
                const newMTopV = topV + this.moveP - this.initP
                // 下一页上升
                this.slide.setActive(newMTopV + 'px');
                if(newMTopV > 0){//向上
                  const bn1 = this.winHeight - (newMTopV);
                  const bn2 = ((this.winHeight - bn1 / 4) / this.winHeight);
                  // 当前页上升并缩小
                  this.slide.setItem(this.slide.newM - 2, bn1 / 4, bn2);
                } else {//向下
                  const bn3 = this.winHeight + (newMTopV);
                  const bn4 = ((this.winHeight - bn3 / 4)/ this.winHeight);
                  if(this.Msize != this.slide.newM){
                    this.slide.setItem(this.slide.newM, bn3/4, bn4);
                  } else {
                    this.slide.setItem(0, bn3/4, bn4);
                  }
                }
                this.initP = this.moveP;
              } else {
                this.moveP = null;	
              }
            } else {
              this.moveP = null;	
            }
          }
		    }
      },
      touchend() {
        if (this.slide.disabled || (this.slide.disabledToLastPage && this.slide.disabledToNextPage)) {
          return;
        }
        //结束控制页面
        this.startM =null;
        this.p_b = false;
        //判断移动的方向
        let move_p;	
        // true为向下滑动
        this.position ? move_p = this.moveP - this.firstP > 0 : move_p = this.firstP - this.moveP > 0 ;
        if(this.move){
          //切画页面(移动成功)
          if( move_p && Math.abs(this.moveP) > 5){
            this.slide.setToTop(this.slide.newM - 1, this.position ? 0 : 1, this.slideName);
          //返回页面(移动失败) fastmode模式下不显示退回
          } else if (!this.slide.fastMode && Math.abs(this.moveP) >=5){	//页面退回去
            if (this.position) {
              // $(".m-page").eq(newM-1).animate({'top':-v_h},100,"easeOutSine");
              this.slide.setToBottomBack(this.slide.newM - 1);
            } else {
              // $(".m-page").eq(newM-1).animate({'top':v_h},100,"easeOutSine")
              this.slide.setToTopBack(this.slide.newM - 1);
            }
          }
        }
        /* 初始化值 */
        this.initP		= null;			//初值控制值
        this.moveP		= null;			//每次获取到的值
        this.firstP		= null;			//第一次获取的值
        this.mousedown	= null;			//取消鼠标按下的控制值
      },
      goToNextPageDirectly() {
        if (this.slide.page_n == 1) {
          this.slide.indexP = false; //true 为不是第一页 false为第一页
        } else {
          this.slide.indexP = true;
        }
        if(this.slide.page_n != this.slide.maxSize){
          this.slide.newM = this.slide.page_n + 1;
        } else {
          return;
        }
        this.slide.setActive(this.v_h);
        this.slide.setToTop(this.slide.newM - 1, 1, this.slideName); // 1 向下滑动 0 向上滑动
      },
      setActive(top) {
        this.showActive = true;
        this.top = top;
      },
      setItem(translateYValue, scaleValue) {
        this.translateY = translateYValue;
        this.scale = scaleValue;
      },
      setToTop() {
        this.animationTop = true;
      },
      setToTopBack() {
        this.animationTopBack = true;
      },
      setToBottomBack() {
        this.animationBottomBack = true;
      },
      setRestart() {
        this.start = true;
      },
      animationend() {
        if (this.animationTop) {
          // 此处监听的是覆盖上去的那个组件
          this.top = 0;
          this.success();
          this.slide.emitPageChangeEnd(this.slideName);
        } else if (this.animationTopBack) {
          // 此处触发监听的是返回的那个组件
          this.sucessBack();
          this.top = 0;
          this.slide.setRestart();
        } else if (this.animationBottomBack) {
          this.successToBottomBack();
          this.top = 0;
          this.slide.setRestart();
        }
      },
      success() {
        this.slide.resetPage();
        this.slide.page_n = this.slide.newM;
        this.start = true;
      },
      sucessBack() {
        this.slide.resetPageBack();
      },
      successToBottomBack() {
        this.slide.resetToBottomPageBack();
      },
      resetShowStatus(value) {
        this.show = value;
      },
      delActiveStatus() {
        this.showActive = false;
        this.translateY = 0;
        this.scale = 1;
        this.animationTop = false;
      },
      delActiveStatusBack() {
        this.showActive = false;
        this.translateY = 0;
        this.scale = 1;
        this.animationTopBack = false;
      },
      delActiveStatusBottomBack() {
        this.showActive = false;
        this.translateY = 0;
        this.scale = 1;
        this.animationBottomBack = false;
      },
      initPage(){
        this.show = true;
        return this.slideName;
      },
    }
  }
</script>
<style lang="less" scoped>

.m-page {
  position:absolute;
  left:0; top:0%;
  height:100%; width:100%;
}
.m-page.show { z-index:10; display:block; }
.m-page.hide { z-index:5; display:none; }
.m-page.active { z-index:15; display:block; }
.m-img {
  width:100%; height:100%;
}

.animationTop {
  animation-name: toTop;
  animation-iteration-count: 1;
  animation-direction: forward;
  animation-duration: .3s;
  animation-timing-function: ease-in-out;
}

.animationTopBack {
  animation-name: toTopBack;
  animation-iteration-count: 1;
  animation-direction: forward;
  animation-duration: .1s;
  animation-timing-function: ease-in-out;
}

.animationBottomBack {
  animation-name: toBottomBack;
  animation-iteration-count: 1;
  animation-direction: forward;
  animation-duration: .1s;
  animation-timing-function: ease-in-out; 
}

@keyframes toTop {
  100% {
    top: 0;
  }
}
@keyframes toTopBack {
  100% {
    top: 100vh;
  }
}
@keyframes toBottomBack {
  100% {
    top: -100vh;
  }
}
</style>