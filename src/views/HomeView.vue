<template>
  <div class="home">
    <div>
      <MainProgram/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import MainProgram from '@/components/MainProgram.vue'

export default {
  name: 'HomeView',
  components: {
    MainProgram
  }
}

</script>

<style>

/****************************************/
.home {
  display: flex ;
  background-color: #0fec55;
  height: 600px;
  justify-content: center;
  align-items: center;
}
</style>
