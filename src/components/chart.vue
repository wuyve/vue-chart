// 聊天窗口
<template>
  <div>
    <!-- 左边的聊天框 -->
    <div class="chart-left">
      <div class="head">
        <img src="/../static/head.png">
        <span>chart 1</span>
      </div>
      <!-- 聊天信息部分 -->
      <div class="mybody">
        <li v-for="item in data" :key='item.key' class="chartItems">
          <span>{{item.date}}</span>
          <!-- 发送信息方发出去的消息 -->
          <div v-if="item.id === 'my'" class="my">
            <div class="mydata">
              <p class="mypop">{{item.data}}</p>
              <img class="myIcon" src="/../static/myIcon.png">
            </div>
          </div>
          <!-- 接收的消息 -->
          <div v-if="item.id === 'friends'" class="friends">
            <div class="friendsData">
              <img class="friendsIcon" src="/../static/head.png">
              <p class="frinedspop">{{item.data}}</p>
            </div>
          </div>
        </li>
      </div>
      <!-- 输入内容和发送区 -->
      <div class="send">
        <el-input
        @keyup.enter.exact.native="sendMsg()"
        @keyup.ctrl.enter.native="getKeycode()"
        class="input"
          type="textarea"
          :autosize="{ minRows: 1, maxRows: 2}"
          v-model="textarea">
        </el-input>
        <el-button size="small" class="button" slot="append" :disabled="show" @click="sendMsg()"><img src="/../static/send.png"></el-button>
      </div>
    </div>
    <!-- 右边聊天框 -->
    <div class="chart-right">
      <div  class="head">
        <img src="/../static/myIcon.png">
        <span>chart 2</span>
      </div>
      <!-- 聊天信息部分 -->
      <div class="mybody">
        <li v-for="item in data" :key='item.key' class="chartItems">
          <span>{{item.date}}</span>
          <!-- 发送信息方发出去的消息 -->
          <div v-if="item.id === 'my'" class="friends-right">
            <div class="friendsData-right">
              <p class="frinedspop-right">{{item.data}}</p>
              <img class="friendsIcon-right" src="/../static/myIcon.png">
            </div>
          </div>
          <!-- 接收的消息 -->
          <div v-if="item.id === 'friends'" class="my-right">
            <div class="mydata-right">
              <img class="myIcon-right" src="/../static/head.png">
              <p class="mypop-right">{{item.data}}</p>
            </div>
          </div>
        </li>
      </div>
      <!-- 输入内容和发送区 -->
      <div class="send">
        <el-input
        @keyup.enter.exact.native="sendMsgRight()"
        @keyup.ctrl.enter.native="getKeycodeRight()"
        class="input"
          type="textarea"
          :autosize="{ minRows: 1, maxRows: 2}"
          v-model="textareaRight">
        </el-input>
        <el-button size="small" class="button" slot="append" :disabled="showRight" @click="sendMsgRight()"><img src="/../static/send.png"></el-button>
      </div>
    </div>
  </div>
</template>
<script>
import $ from 'jquery'
export default {
  data () {
    return {
      textarea: '',  // 左边输入框
      textareaRight: '',  // 右边输入框
      show: true,
      showRight: true,
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
    // 加载完毕后消息显示至最底层，srcollTop内的参数根据已存在的消息条数计算
    let aa = $('.mybody').scrollTop(500)
    // console.log(aa)
  },
  updated () {
    // 发送数据后，视图更新时滚动条显示最底部
    // srcollTop内的参数根据已存在的消息条数计算
    $('.mybody').scrollTop(2500 * this.key)
    // 右边输入框为空时，按钮为禁用状态
    if (this.textareaRight === '') {
      this.showRight = true
    } else {
      this.showRight = false
    }
    // 左边输入框为空时，按钮为禁用状态
    if (this.textarea === '') {
      this.show = true
    } else {
      this.show = false
    }
  },
  methods: {
    // 左边聊天框发送消息函数
    sendMsg: function () {
      // 检测URL
      /* eslint-disable */
      let reg = /https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)/
      if (this.textarea.match(reg)) {
        let url = this.textarea.match(reg)[0]
        console.log(url)
      }
      console.log(this.textarea)
      // 发送字数不能超过3420字（同QQ）
      if (this.textarea.length >= 3420) {
        this.$message({
          message: '字数过长，不能超过3420字',
          type: 'warning',
          center: true
        })
      } else {
        let obj = {}
        obj.data = this.textarea.replace('\n', '\n')
        obj.date = new Date().toLocaleString()
        obj.id = 'my'
        obj.key = this.key++
        this.data.push(obj)
        this.textarea = ''
        this.show = true
      }
    },
    // 右边聊天框发送信息
    sendMsgRight: function () {
      console.log(this.textareaRight)
      // 发送字数不能超过3420字（同QQ）
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
        this.showRight = true
      }
    },
   // 左边输入框换行
    getKeycode: function () {
      this.textarea = this.textarea + '\n'
    },
    // 右边输入框换行
    getKeycodeRight: function () {
      this.textareaRight = this.textareaRight + '\n'
    }
  }
}
</script>
<style scoped src="../../static/chart.css">
</style>