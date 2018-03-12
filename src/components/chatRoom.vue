<template>
    <div class="row justify-content-center">
        <div class="col-md-8 ">
            <h2>使用者：{{userName}}</h2>
            <ul style="list-style-type:none" class="msssage-area text-left">
                <li v-for="message in messages">
                    <span class="h5">{{message.userName}}: </span><span class="h4">{{message.message}}</span><span class="h6">  {{message.timeStamp}}</span> 
                </li>
            </ul>
        </div>
        <div class="col-md-8 text-left ">
            <div class="input-group mb-3">
                <input type="text" class="form-control" placeholder="請輸入訊息" aria-label="請輸入訊息" aria-describedby="basic-addon2" v-model="sendMessageInfo" v-on:keyup.enter="sendMessage" >
                    <div class="input-group-append">
                        <button class="btn btn-secondary" type="button" v-on:click="sendMessage">發送訊息</button>
                    </div>
            </div>
            <button type="button" class="btn btn-secondary " data-toggle="modal" data-target="#setUserNameModal">
                重新設定姓名
            </button>
        </div>
        
        <!-- <button type="button" class="btn btn-primary" v-on:click="sendMessage">test button</button> -->

        
        <!-- Modal -->
        <div class="modal fade" id="setUserNameModal" tabindex="-1" role="dialog" aria-labelledby="setUserNameModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="setUserNameModalLabel">請輸入姓名</h5>
                        <button type="button" class="close"  aria-label="Close" v-on:click="toggleUsernameModal">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <form id="userNameForm" action="#">
                            <div class="form-group">
                                <input type="text" class="form-control" placeholder="e.g :Terry" aria-label="e.g :Terry" aria-describedby="basic-addon2" v-model="userName" v-on:keyup.enter="toggleUsernameModal" required >
                            </div>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" v-on:click="toggleUsernameModal">Close</button>
                        <!-- <button type="button" class="btn btn-primary">Save changes</button> -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import * as firebase from "firebase";
import $ from "jquery";
import moment from "moment";
moment.locale("zh-tw");

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
      userName: "",
      sendMessageInfo: "",
      messages: []
    };
  },
  methods: {
    sendMessage() {
      if (this.sendMessageInfo && this.sendMessageInfo.length > 0) {
        msgRef.push({
          userName: this.userName,
          message: this.sendMessageInfo,
          timeStamp: moment().format('LT')
        });
        this.sendMessageInfo = "";
      }
    },
    toggleUsernameModal() {
      if (this.userName && this.userName.length > 0) {
        $("#setUserNameModal").modal("toggle");
      } else {
        $("#userNameForm").submit();
      }
    }
  },
  mounted() {
    $("#setUserNameModal").modal({
      keyboard: false,
      backdrop: "static"
    });
    $("#setUserNameModal").modal("show");
    const vm = this;
    msgRef.on("value", function(snapshot) {
      vm.messages = snapshot.val();
    });
  }
};
</script>

<style scoped>
.msssage-area li {
  padding-bottom: 10px;
}
</style>