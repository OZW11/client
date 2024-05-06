<template>
  <div class="home">
    <div>
      <HelloWorld  msg="Welcome to Your Vue.js App"/>
    </div>
    <div>
      <button class="button_" @click="wst_img">wst_img</button>
      <button class="button_" @click="chat">chat</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import '../socket/socket.js'


import anime from 'animejs/lib/anime.es.js';


export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  methods: {
    wst_img: ()=>{
      DATA_BUFFER=JSON.stringify({ 'message':{
        'height': 768,
        'width': 576,
        'prompt': '一只梵高画的猫',
        'styleConfig': '55c682d5eeca50d4806fd1cba3628781'
        }})
        socket.send('wst_img');

        var button_=document.querySelectorAll('.button_')
        
        anime({
          targets: button_,
          translateY: -100,
          duration: 500
        });

    },
    'chat':()=>{
    DATA_BUFFER=JSON.stringify({ 'message':  [
        {
            "role": "user",
            "content": "你能告诉我什么是人工智能吗？"
        },
        {
            "role": "assistant",
            "content": "人工智能是指由人制造出来的系统能够模拟人类智能行为的技术。它包括机器学习、自然语言处理、计算机视觉等多个领域。"
        },
        {
            "role": "user",
            "content": "那么机器学习又是什么呢？"
        },
        {
            "role": "assistant",
            "content": "机器学习是人工智能的一个分支，它让计算机能够通过数据分析和模式识别来学习和改进，而无需每一步都进行明确的编程。"
        },
        {
            "role": "user",
            "content": "机器学习有哪些应用呢？"
        }
        ]})
    socket.send('chat');    
    var button_=document.querySelectorAll('.button_')
        anime({
          targets: button_,
          translateY: 100,
          duration: 500
        });  
    }
    
  },
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
