<template>
  <button id="button">{{ MyName }}</button>
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

<style>
    #button {
        width: 200px;
        height: 40px;
        background-color: hsl(49deg 98% 60%);
        font-family: inherit;
        padding: 0.6em 1.3em;
        font-weight: 900;
        font-size: 18px;
        border: 3px solid rgb(2, 255, 188);
        border-radius: 0.4em;
        box-shadow: 0.1em 0.1em rgb(112, 137, 66);
        cursor: pointer;
    }

      #button:hover {
        transform: translate(-0.05em, -0.05em);
        box-shadow: 0.15em 0.15em;
      }

      #button:active {
        transform: translate(0.05em, 0.05em);
        box-shadow: 0.05em 0.05em;
      }
</style>