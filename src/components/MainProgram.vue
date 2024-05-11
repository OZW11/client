<template>
  <div class="but">
    <Mybutton ref="chat_button" MyName='chat' MyEvent='chat' @chat="chat"/>
    <Mybutton ref="wst_img_button" MyName='wst_img' v-if="buttonState" MyEvent="wst_img_buttonClick" @wst_img_buttonClick="chooseEnglishWord_character" />
    <Mybutton ref="wst_img_button_choose_English_words_Chicken" v-if="buttonState" MyName='Chicken' MyEvent="chooseEnglishWord_character" @chooseEnglishWord_character="chooseEnglishWord_place('Chicken')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Cow" MyName='Cow' v-if="buttonState" MyEvent="chooseEnglishWord_character" @chooseEnglishWord_character="chooseEnglishWord_place('Cow')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Lion" MyName='Lion' v-if="buttonState" MyEvent="chooseEnglishWord_character" @chooseEnglishWord_character="chooseEnglishWord_place('Lion')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Forest" MyName='Forest' v-if="buttonState" MyEvent="chooseEnglishWord_place" @chooseEnglishWord_place="chooseEnglishWord_event('Forest')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Lawn" MyName='Lawn' v-if="buttonState" MyEvent="chooseEnglishWord_place" @chooseEnglishWord_place="chooseEnglishWord_event('Lawn')"/>   
    <Mybutton ref="wst_img_button_choose_English_words_Dance" MyName='Dance' v-if="buttonState" MyEvent="chooseEnglishWord_event" @chooseEnglishWord_event="chooseEnglishWord_final('Dance')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Sing" MyName='Sing' v-if="buttonState" MyEvent="chooseEnglishWord_event" @chooseEnglishWord_event="chooseEnglishWord_final('Sing')"/>    
    <Mybutton ref="wst_img_button_choose_English_words_Play" MyName='Play' v-if="buttonState" MyEvent="chooseEnglishWord_event" @chooseEnglishWord_event="chooseEnglishWord_final('Play')"/>  
    <img v-if="Myimg.state" :src="Myimg.url"  alt="NONE" width="300" height="300">
  </div>
</template>

<script>
import anime from 'animejs'
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
      TRANSFER_SIZE: 1024,
      img_mess: {'character':'','place':'','event':''},
      buttonState: true
    }
  },
  methods: {
    // ***************交互动画方法*****************
    'MybuttonAnimationMove': function(MyName,MyEvent,tX,Duration){
      let Mybutton = this.$refs[MyName];
      
      anime({
        targets: Mybutton.$el,
        translateX: tX,
        duration: Duration,
        easing: 'easeOutExpo',
        complete: function() {
        Mybutton.$emit(MyEvent);
        }
      });
    },
    // ***************通讯与用户事件逻辑处理方法*****************
    'wst_img': function(mess){
      this.DATA_BUFFER=JSON.stringify({ 'message':{
        'height': 768,
        'width': 576,
        'prompt': mess,
        'styleConfig': '8fe3d641be3e589dad231dc6c3b1429a'
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
    //**********************逻辑处理方法**************************************************************************
    chooseEnglishWord_character: function(mess){
      console.log(this.img_mess);
      this.MybuttonAnimationMove("wst_img_button","anyevent",400,1500);
      this.MybuttonAnimationMove("chat_button","anyevent",-400,1500);
      console.log("chooseEnglishWord_character");
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Chicken","anyevent",40,1400);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Cow","anyevent",40,1800);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lion","anyevent",40,400);
    },
    chooseEnglishWord_place: function(mess){
      this.img_mess['character'] = mess;
      console.log(this.img_mess['character']);
      console.log('chooseEnglishWord_place');
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Chicken","anyevent",240,400);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Cow","anyevent",-240,1000);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lion","anyevent",240,1600);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Forest","anyevent",30,400);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lawn","anyevent",35,800);
    },
    chooseEnglishWord_event: function(mess){
      this.img_mess['place'] = mess;
      console.log(this.img_mess['place']);
      console.log('chooseEnglishWord_event');
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Forest","anyevent",-240,1600);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lawn","anyevent",350,800);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Dance","anyevent",-20,800);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Sing","anyevent",35,1600);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Play","anyevent",10,400);   
    },
    chooseEnglishWord_final: function(mess){
      this.img_mess['event'] = mess;
      console.log(this.img_mess['event']);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Dance","anyevent",240,800);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Sing","anyevent",-240,1600);
      this.MybuttonAnimationMove("wst_img_button_choose_English_words_Play","anyevent",240,400);   
      let StringSend="用一下关键词画一幅小朋友喜欢的画 "+this.img_mess['character'] +" "+this.img_mess['place']+" "+this.img_mess['event'];
      console.log(StringSend);
      this.wst_img(StringSend);
    }
  },

  
  mounted() {
    //
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Chicken","anyevent",-500,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Cow","anyevent",500,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lion","anyevent",-500,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Forest","anyevent",240,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Lawn","anyevent",240,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Dance","anyevent",-500,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Sing","anyevent",240,1);
    this.MybuttonAnimationMove("wst_img_button_choose_English_words_Play","anyevent",240,1);    
    //
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
            this.Myimg.state=true;
            this.buttonState=false;
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