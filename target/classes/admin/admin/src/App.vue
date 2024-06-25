<template>
  <div id="app" class="">
    <router-view></router-view>
    <text></text>
  </div>

</template>
<script>
import text from "./text.vue";
export default {
  name: "app",
  components:{
    text
  }

};


</script>

<style lang="scss">
*{
  padding: 0;
  margin:0;
  box-sizing: border-box;
}
html,body{
  width: 100%;
  height: 100%;
}
#app{
  height:100%;
}
body {
  padding: 0;
  margin: 0;

}
</style>
