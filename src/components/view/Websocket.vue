<template xmlns="http://www.w3.org/1999/html">
  <div>
    自己的ID： <input type="text" v-model="userId" placeholder="输入自己的ID"><br><br>
    好友的ID：<input type="text" v-model="toUserId" placeholder="对方的ID"><br><br>
    需要发送的信息：<input type="text" v-model="message" placeholder="需要发送的消息"><br><br>
    <p>【操作】：<div><button @click="openSocket">开启socket</button></div>
    <p>【操作】：<div><button @click="sendMessage">发送消息</button></div>
  </div>
</template>

<script>
  export default {
    name: "WebSocket",
    data(){
      return{
        userId:'',
        toUserId:'',
        message:''
      }
    },
    methods: {
      openSocket: function () {
        if (typeof (WebSocket) == "undefined"){
          console.log("浏览器不支持")
        }else{
          console.log("浏览器支持")
          console.log("您的id为"+this.userId)
        }

        // $("#userId").val()
        var socketUrl="http://localhost:8089/demo/imserver/"+this.userId;
        socketUrl=socketUrl.replace("https","ws").replace("http","ws");
        console.log("链接地址为"+socketUrl);
        this.socket = new WebSocket(socketUrl);
        this.socket.onopen = function() {
          console.log("websocket已打开");
          //socket.send("这是来自客户端的消息" + location.href + new Date());
        };
        //获得消息事件
        this.socket.onmessage = function(e) {
          console.log(e.data);
          //发现消息进入    开始处理前端触发逻辑
        };
        //关闭事件
        this.socket.onclose = function() {
          console.log("websocket已关闭");
        };
        //发生了错误事件
        this.socket.onerror = function() {
          console.log("websocket发生了错误");
        }
      },
      sendMessage:function () {
        if (typeof (WebSocket)=="undefined"){
          console.log("浏览器不支持");
        }else {
          console.log("支持");
         /* console.log('{"toUserId":"'+$("#toUserId").val()+'","contentText":"'+$("#contentText").val()+'"}');
          this.socket.send('{"toUserId":"'+$("#toUserId").val()+'","contentText":"'+$("#contentText").val()+'"}');*/
          console.log('{"向好友发送信息":"'+this.toUserId+'","contentText":"'+this.message+'"}');
          this.socket.send('{"toUserId":"'+this.userId+'","contentText":"'+this.message+'"}')
        }
      }

    }
  }
</script>

<style scoped>

</style>
