// 聊天窗口
<template>
  <div>
    <div class="chart-left">
      <div class="head">
        <img src="/../static/head.png">
        <span>chart 1</span>
      </div>
      <div class="mybody">
        <li v-for="item in data" :key='item.key' class="chartItems">
          <span>{{item.date}}</span>
          <div v-if="item.id === 'my'" class="my">
            <div class="mydata">
              <p class="mypop">{{item.data}}</p>
              <img class="myIcon" src="/../static/myIcon.png">
            </div>
          </div>
          <div v-if="item.id === 'friends'" class="friends">
            <div class="friendsData">
              <img class="friendsIcon" src="/../static/head.png">
              <p class="frinedspop">{{item.data}}</p>
            </div>
          </div>
        </li>
      </div>
      <div class="send">
        <el-input
        @keyup.enter.exact.native="sendMsg()"
        @keyup.ctrl.enter.native="getKeycode()"
        class="input"
          type="textarea"
          :autosize="{ minRows: 1, maxRows: 2}"
          v-model="textarea">
        </el-input>
        <el-button size="small" class="button" slot="append" @click="sendMsg()"><img src="/../static/send.png"></el-button>
      </div>
    </div>
    <div class="chart-right">
      <div  class="head">
        <img src="/../static/myIcon.png">
        <span>chart 2</span>
      </div>
      <div class="mybody">
        <li v-for="item in data" :key='item.key' class="chartItems">
          <span>{{item.date}}</span>
          <div v-if="item.id === 'my'" class="friends-right">
            <div class="friendsData-right">
              <p class="frinedspop-right">{{item.data}}</p>
              <img class="friendsIcon-right" src="/../static/myIcon.png">
            </div>
          </div>
          <div v-if="item.id === 'friends'" class="my-right">
            <div class="mydata-right">
              <img class="myIcon-right" src="/../static/head.png">
              <p class="mypop-right">{{item.data}}</p>
            </div>
          </div>
        </li>
      </div>
      <div class="send">
        <el-input
        @keyup.enter.exact.native="sendMsgRight()"
        @keyup.ctrl.enter.native="getKeycodeRight()"
        class="input"
          type="textarea"
          :autosize="{ minRows: 1, maxRows: 2}"
          v-model="textareaRight">
        </el-input>
        <el-button size="small" class="button" slot="append" @click="sendMsgRight()"><img src="/../static/send.png"></el-button>
      </div>
    </div>
  </div>
</template>
<script>
// import bus from '@/components/eventBus'
// import Routers from '@/router'
import $ from 'jquery'
export default {
  data () {
    return {
      textarea: '',
      textareaRight: '',
      data: [
        {
          id: 'my',
          date: '2019/10/09 13:20:55',
          data: '你好啊啊啊aiufhiuwshgftiwsnvfsnkjdnsajfbhrwyuegruybhbjhb',
          key: 1
        },
        {
          id: 'friends',
          date: '2019/10/09 13:28:55',
          data: '你也好啊',
          key: 2
        }
      ],
      key: 6
    }
  },
  mounted () {
    let aa = $('.mybody').scrollTop(500)
    console.log(aa)
  },
  updated () {
    $('.mybody').scrollTop(2500 * this.key)
  },
  methods: {
    sendMsg: function () {
      /* eslint-disable */
      let reg = /https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)/
      if (this.textarea.match(reg)) {
        let url = this.textarea.match(reg)[0]
        console.log(url)
      }
      console.log(this.textarea)
      if (this.textarea === '') {
        this.$message({
          message: '发送不能为空',
          type: 'warning',
          center: true
        })
      } else if (this.textarea !== '') {
        let obj = {}
        obj.data = this.textarea.replace('\n', '\n')
        obj.date = new Date().toLocaleString()
        obj.id = 'my'
        obj.key = this.key++
        this.data.push(obj)
        this.textarea = ''
      }
    },
     sendMsgRight: function () {
      let srcoll = document.documentElement.scrollTop
      console.log(srcoll)
      console.log(this.textareaRight)
      if (this.textareaRight === '') {
        this.$message({
          message: '发送不能为空',
          type: 'warning',
          center: true
        })
      } else if (this.textareaRight !== '') {
        if (this.textareaRight.length >= 3420) {
          this.$message({
            message: '字数过长，不能超过3420字',
            type: 'warning',
            center: true
          })
        } else {
          let obj = {}
          obj.data = this.textareaRight.replace('\n', '\n')
          obj.date = new Date().toLocaleString()
          obj.id = 'friends'
          obj.key = this.key++
          this.data.push(obj)
          this.textareaRight = ''
        }
      }
    },
    getKeycode: function () {
      this.textarea = this.textarea + '\n'
    },
    getKeycodeRight: function () {
      this.textarea = this.textarea + '\n'
    },
    bottomshow: function () {
      let count = 0
      let interval = setInterval(() => {
        if (count > 200) {
          clearInterval(interval)
        }
        count++
        if ($('.mybody').scrollTop !== $('.mybody').scrollHeight) {
          $('.mybody').scrollTop = $('.mybody').scrollHeight
          console.log($('.mybody').scrollTop)
        }
        if ($('.mybody').scrollTop === $('.mybody').scrollHeight) {
          clearInterval(interval)
        }
      }, 0)
    }
  }
}
</script>
<style scoped>
  .chart-left {
    width: 640px;
    height: 410px;
    border: 1px solid black;
    border-radius: 15px;
    margin-left: 19px;
  }
  .head {
    margin-top: 10px;
  }
  .mybody {
    height: 300px;
    overflow-y: auto;
    background: #F1F3F4;
  }
  .input {
    width: 557px;
    height: 52px;
    position: relative;
    left: 0px;
    bottom: -5px;
  }
  .send {
    height: 23px;
  }
  .button {
    position: relative;
    top: 7px;
    left: 0px;
  }
  .chartItems {
    width: 100%;
    padding: 0;
    margin-top: 10px;
    list-style: none;
    margin-bottom: -30px;
  }
  .my {
    width: 40%;
    margin-left: 370px;
  }
  .myIcon {
    position: relative;
    bottom: 40px;
    left: 104px;
  }
  .friends {
    width: 40%;
  }
  .mydata {
  }
  .friendsData {
  }
  .mypop {
    padding: 3px;
    border: 1px solid #ccc;
    border-radius: 0 10px;
    background: #B0DA91;
    position: relative;
    right: 45px;
    /* width:auto; */
    width:fit-content;
    height:auto;
    text-align: left;
    word-break:break-all;
    white-space: pre-line;
  }
  .frinedspop {
    padding: 3px;
    border: 1px solid #ccc;
    border-radius: 10px 0;
    position: relative;
    left: 44px;
    top: -40px;
    width:auto;
    height:auto;
    text-align: left;
    word-break:break-all;
    white-space: pre-line;
  }
  .friendsIcon {
    position: relative;
    left: -100px;
    top: 17px;
  }
  .chart-right {
    width: 640px;
    height: 410px;
    border: 1px solid black;
    border-radius: 15px;
    margin: -411px 0px 0px 692px;
  }
  .friends-right {
    width: 40%;
  }
  .my-right {
    width: 40%;
    margin-left: 370px;
  }
  .friendsData-right {
  }
  .mydata-right {}
  .frinedspop-right {
    padding: 3px;
    border: 1px solid #ccc;
    border-radius: 10px 0;
    position: relative;
    left: 46px;
    top: 0px;
    width: auto;
    height: auto;
    text-align: left;
    word-break: break-all;
    white-space: pre-line;
  }
  .mypop-right {
    padding: 3px;
    border: 1px solid #ccc;
    border-radius: 0 10px;
    background: #B0DA91;
    position: relative;
    right: 39px;
    bottom: 35px;
    width: auto;
    height: auto;
    text-align: left;
    word-break: break-all;
    white-space: pre-line;
  }
  .friendsIcon-right {
    position: relative;
    left: -100px;
    top: -44px;
  }
  .myIcon-right {
    position: relative;
    bottom: -25px;
    left: 104px;
  }
</style>