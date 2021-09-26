<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/slide.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  width: 100%;
  height: 100%;
}
* { margin: 0; outline: 0; padding: 0; font-size: 100%; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); }
article,aside,dialog,footer,header,section,footer,nav,figure,menu{display:block}
a { text-decoration: none; -webkit-tap-highlight-color: rgba(0, 0, 0, 0.35); }
html { height: 100%; -webkit-text-size-adjust: 100%; -ms-text-size-adjust: 100%; }
body { margin: 0; padding: 0; width: 100%; height: 100%; min-height: 100%; font-family:"\5FAE\8F6F\96C5\9ED1","\5b8b\4f53", Arial, Helvetica, sans-serif; font-size: 62.5%; line-height: 1.231; -webkit-touch-callout: none; display: -webkit-box; -webkit-box-orient: vertical; -webkit-box-align: stretch; position: relative; }
img { -ms-interpolation-mode: bicubic; vertical-align: middle; border: 0 none;max-width: 100%;height: auto;width: auto\9; /* ie8 */}
img:not([src*="/"]) { display: none; }
table { border-collapse: collapse; border-spacing: 0; width: 100%; }
th, td, caption { vertical-align: middle; }
textarea { resize: none; border: 0; padding: 8px 0; border-radius: 0;}
ul,li { list-style: none; margin:0; padding:0; }
h1,h2,h3,h4,h5,h6 {margin:0; padding:0; font-weight:normal;}
</style>
