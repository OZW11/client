<template>
  <div>
    <button @click='whenclick'>{{MyName}}</button>    
  </div>
</template>

<script>
export default {
  name: 'Mybutton',
  props: {
    MyName: '',
    MyEvent: '',
  },
  methods: {
    whenclick(){
      this.$emit(this.MyEvent)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
