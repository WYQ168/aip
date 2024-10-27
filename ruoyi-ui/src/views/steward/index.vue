<template>
  <div id="chatbox" class="chatbox">
    <!--    <div class="question">-->
    <!--      {{ userQuestion }}-->
    <!--    </div>-->

    <div class="messages" ref="messages">
      <div v-for="(msg, index) in messages" :key="index" :class="['message', msg.type]">
        {{ msg.content }}
      </div>
    </div>

    <div class="tips">
      <button @click="quickSend('分析下最近一周CEA价格情况')">分析下最近一周CEA价格情况</button>
      <button @click="quickSend('写一个本网以上的CEA月报')">写一个上个月的CEA月报</button>
      <button @click="quickSend('这个月的价格最高和最低是多少？')">这个月的价格最高和最低是多少？</button>
    </div>

    <div class="input-container">
      <el-input
        v-model="input"
        @keyup.enter="sendMessage"
        placeholder="请在此输入您的问题..."
      ></el-input>
      <el-button type="primary" @click="sendMessage">发送</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        input: '',
        messages: [],
        botResponse: '这是系统回复的固定内容',
        userQuestion: '' // 用户输入的问题
      };
    },
    methods: {
      sendMessage() {
        if (!this.input.trim()) return;

        // 设置用户问题
        this.userQuestion = this.input;

        // 添加用户消息
        this.messages.push({
          content: this.input,
          type: 'user',
        });

        // 清空输入框
        this.input = '';

        // 模拟 Bot 的延迟回复
        setTimeout(() => {
          this.messages.push({
            content: this.botResponse,
            type: 'bot',
          });
          this.$nextTick(() => {
            this.scrollToBottom(); // 滚动到最底部
          });
        }, 1000);
      },
      quickSend(question) {
        this.input = question;
        this.sendMessage(); // 发送消息
      },
      scrollToBottom() {
        this.$refs.messages.scrollTop = this.$refs.messages.scrollHeight;
      },
    },
  };
</script>

<style scoped>
  .chatbox {
    /*max-width: 600px; !* 最大宽度 *!*/
    /*margin: 20px auto; !* 居中显示 *!*/
    width: 100%;
    height: 100%;
    border: 1px solid #eaeaea;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    font-family: Arial, sans-serif;
    background: #ffffff;
  }

  .question {
    padding: 15px;
    background-color: #3498db; /* 标题的背景颜色 */
    color: white;
    font-size: 16px;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
  }

  .messages {
    flex: 1;
    padding: 10px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 5px;
    background-color: #f0f0f0;
  }

  .message {
    border-radius: 8px;
    padding: 10px;
    max-width: 75%;
    word-wrap: break-word; /* 防止长单词溢出 */
  }

  .user {
    align-self: flex-end;
    background-color: #3498db;
    color: white;
  }

  .bot {
    align-self: flex-start;
    background-color: #f1f1f1;
    color: #333;
  }

  .tips {
    padding: 10px;
    background-color: #f9f9f9;
    border-bottom: 1px solid #eaeaea;
    font-size: 25px;
  }

  .tips button {
    margin: 0 5px;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    background-color: #3498db;
    color: white;
    cursor: pointer;
  }

  .tips button:hover {
    background-color: #2980b9;
  }

  .input-container {
    border-top: 1px solid #eaeaea;
    display: flex;
    align-items: center;
    padding: 10px;
    background-color: #fff;
  }

  .input-container .el-input {
    flex: 1;
    margin-right: 10px;
  }

  .input-container .el-button {
    height: 36px;
  }
</style>
