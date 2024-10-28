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
      <button @click="quickSend('分析下最近一周CEA价格情况',1)">分析下最近一周CEA价格情况</button>
      <button @click="quickSend('写一个本月以上的CEA月报',2)">写一个上个月的CEA月报</button>
      <button @click="quickSend('这个月的价格最高和最低是多少？',3)">这个月的价格最高和最低是多少？</button>
    </div>

    <div class="input-container">
      <el-input
        v-model="input"
        @keyup.enter="sendMessage"
        placeholder="请在此输入您的问题..."
      ></el-input>
      <el-button type="primary" @click="sendMessage(input)">发送</el-button>
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
      sendMessage(str) {
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
            content: str,
            type: 'bot',
          });
          this.$nextTick(() => {
            this.scrollToBottom(); // 滚动到最底部
          });
        }, 1000);
      },
      quickSend(question, index) {
        this.input = question;
        let str = ""
        switch (index) {
          case 1:
            str = "我查询了最近一周的CEA价格情况，以下是具体数据:\n" +
              "·2024年10月25日:开盘价104.03元/吨，最高价104.3元/吨，最低价103.0元/吨，收盘价104.25元/吨，成交量390666吨，成交额40658535.4元。\n" +
              "·2024年10月24日:开盘价104.01元/吨，最高价104.14元/吨，最低价104.01元/吨，收盘价104.04\n" +
              "元/吨，成交量386268吨，成交额40039863.09元。\n" +
              "·2024年10月23日:开盘价104.04元/吨，最高价104.14元/吨，最低价104.0元/吨，收盘价104.1元/\n" +
              "吨，成交量546800吨，成交额55927070.0元。\n" +
              "·2024年10月22日:开盘价104.06元/吨，最高价104.33元/吨，最低价104.0元/吨，收盘价104.13元/吨，成交量1376416吨，成交额137825583.49元。\n" +
              "·2024年10月21日:开盘价103.2元/吨，最高价104.53元/吨，最低价103.2元/吨，收盘价104.14元/\n" +
              "吨，成交量317563吨，成交额32463870.5元。\n" +
              "从数据来看，CEA价格在这一周内保持稳定，价格波动不大。值得注意的是，10月22日的成交量大幅增加，达到了1376416吨，可能是市场对于碳排放权交易的关注度有所上升。\n" +
              "有趣的知识点:全国碳排放权交易市场是我国为了控制温室气体排放而建立的市场机制，通过碳排放权的买卖，促进企业减少排放。";
            break;
          case 2:
            str = "碳配额，全称为碳排放配额(Carbon Emission Allowance，简称CEA)，是指在国家碳排放权交易体系中，政府根据碳排放控制目标和行业基准，分配给企业的一定时期内的二氧化碳排放许可量。企业拥有这些配额，就相当于拥有了一定数量的二氧化碳排放权利。如果企业的实际排放量低于分配的配额，可以将剩余的配额在碳市场上出售;如果企业的排放量超过了分配的配额，则需要从市场上购买额外的配额，或者使用国家核证自愿减排量(CCER)进行抵消。\n" +
              "碳配额交易是碳排放权交易市场的基本组成部分，旨在通过市场机制促进碳排放的减少，以更经济有效的方式实现国家的减排目标。在中国，全国碳排放权交易市场于2021年启动，碳配额成为交易的主要产品之一。";
            break;
          case 3:
            str="最高10000，最低5000";
            break;
        }
        this.sendMessage(str); // 发送消息
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
    color: #333;
    background-color: white;
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
