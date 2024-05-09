<template>
  <div id="app">
      <comic></comic>
      <home-view></home-view>
  </div>
</template>



<script>
import comic from './components/Comic.vue'
import HomeView from './views/HomeView.vue';
export default {
  name:'App',
  components:{
    comic,
    HomeView
  }
}
</script>


<style>

</style>
