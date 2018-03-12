<template>
    <div>
        <h2>您好：{{userName}}</h2>
        <ul>
            <li v-for="message in messages">
                {{message.userName+" "+message.message+" "+message.timeStamp}}
            </li>
        </ul>
        
        <div class="input-group mb-3">
            <input type="text" class="form-control" placeholder="請輸入訊息" aria-label="請輸入訊息" aria-describedby="basic-addon2" v-model="sendMessageInfo" v-on:keyup.enter="sendMessage">
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button" v-on:click="sendMessage">發送訊息</button>
            </div>
        </div>
        

        <!-- Button trigger modal -->
        <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#setUserNameModal">
            重新設定姓名
        </button>
        <!-- <button type="button" class="btn btn-primary" v-on:click="sendMessage">test button</button> -->

        
        <!-- Modal -->
        <div class="modal fade" id="setUserNameModal" tabindex="-1" role="dialog" aria-labelledby="setUserNameModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="setUserNameModalLabel">請輸入姓名</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="input-group mb-3">
                            <input type="text" class="form-control" placeholder="e.g :Terry" aria-label="e.g :Terry" aria-describedby="basic-addon2" v-model="userName">
                            <!-- <div class="input-group-append">
                                <button class="btn btn-outline-secondary" type="button">設定</button>
                             </div> -->
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <!-- <button type="button" class="btn btn-primary">Save changes</button> -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import * as firebase from 'firebase';
import $ from 'jquery';
import moment from 'moment';
moment.locale('zh-tw');
var app = firebase.initializeApp({
  apiKey: "AIzaSyCpyu35xCLghjXmwHj0LtSjYqZPLgECFfI",
  authDomain: "chatroom-fb7d1.firebaseapp.com",
  databaseURL: "https://chatroom-fb7d1.firebaseio.com",
  projectId: "chatroom-fb7d1",
  storageBucket: "chatroom-fb7d1.appspot.com",
  messagingSenderId: "588630522077"
});

const msgRef = firebase.database().ref("/messages/");

export default {
  name: "chatRoom",
  data() {
    return {
        userName:'',
        sendMessageInfo:'',
        messages: []
    };
  },
  methods: {
    sendMessage() {
      msgRef.push({
        userName: this.userName,
        message: this.sendMessageInfo,
        // 取得時間，這裡的vm.getTime()就是method中的getTime
        timeStamp: moment().format('LLLL')
      });
      this.sendMessageInfo="";
    }
  },
  mounted () {
    $('#setUserNameModal').modal('show');
    const vm = this;
    msgRef.on('value', function(snapshot) {
      vm.messages = snapshot.val();
    })
  }
};
</script>

<style scoped>

</style>