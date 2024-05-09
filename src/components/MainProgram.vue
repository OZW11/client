<template>
  <div class="but">
    <Mybutton MyName='chat' MyEvent='chat' @chat="chat"/>
    <Mybutton MyName='wst_img' MyEvent="wst_img" @wst_img="wst_img"/>
    <img :src="Myimg.url" alt="NONE" width="300" height="300">
    <Mysocket />
  </div>
</template>

<script>
import Mybutton from './Mybutton.vue'
export default {
  name: 'MainProgram',
  components: {
    Mybutton,
  },
  data () {
    return {
      Myimg:{ url:'' ,state: false},
      socket: null,
      DATA_BUFFER: '',
      TRANSFER_SIZE: 1024
    }
  },
  methods: {
    'wst_img': function(){
      this.DATA_BUFFER=JSON.stringify({ 'message':{
        'height': 768,
        'width': 576,
        'prompt': '一只梵高画的猫',
        'styleConfig': '55c682d5eeca50d4806fd1cba3628781'
        }})
        this.socket.send('wst_img');

    },
    'chat':function(){
      this.DATA_BUFFER=JSON.stringify({ 'message':  [
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
    this.socket.send('chat');      
    },
  },
  mounted() {
  this.socket =new WebSocket('ws://127.0.0.1:8765');
  this.DATA_BUFFER='';
  this.TRANSFER_SIZE=1024;
  console.log(this.DATA_BUFFER)
  // 连接建立时触发  
  this.socket.onopen = function(event) {  
      console.log('Connected to server!');
  }; 
  // 接收到来自服务器的消息时触发  
  this.socket.onmessage = (event) => { 
    const MY_SOCKET_sender = () =>{
      // 大量数据分片发送，在使用时先对全局变量DATA_BUFFER赋值，这个函数就会不断发送消息直到完成
      // 完成发送后，会向服务器返回'message_have_sent_done'
        if(this.DATA_BUFFER.length > this.TRANSFER_SIZE)
        {
            this.socket.send(this.DATA_BUFFER.slice(0,this.TRANSFER_SIZE));
            this.DATA_BUFFER=this.DATA_BUFFER.slice(this.TRANSFER_SIZE,this.DATA_BUFFER.length);
        }
        else
        {
            this.socket.send(this.DATA_BUFFER);
            this.DATA_BUFFER='';
        }
    }
      console.log('Message from server: ', event.data); 
      // 从服务器收到READY__代表正在经行大量数据传输，执行对应逻辑
      if(event.data == 'READY__' && this.DATA_BUFFER == '')
      {
        this.socket.send('message_have_sent_done');
      }
      else if(event.data == 'READY__')
      {
        MY_SOCKET_sender();
      }
      if(event.data.length > 11 )
      {
        if(event.data.slice(0,11) == 'CHAT_RETURN')
        {
          console.log(event.data.slice(11,event.data.length));
        }
      }
      if(event.data.length > 14 )
      {
          if(event.data.slice(0,14) == 'WST_IMG_RETURN')
          {
            console.log(event.data.slice(14,event.data.length));
            this.Myimg.url=event.data.slice(14,event.data.length);
          }
      }
    
      };  

  }
}

</script>

<style>
  .but{
    
  }
   #img{
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 5px;
   } 
</style>