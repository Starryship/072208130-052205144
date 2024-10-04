<template>
  <view class="chat-app">
    <!-- 左侧联系人列表 -->
    <view class="contact-list">
      <view
        v-for="contact in contacts"
        :key="contact.id"
        :class="['contact-item', { active: contact.id === currentContact.id }]"
        @click="selectContact(contact)"
      >
        <img :src="contact.avatar" alt="头像" class="avatar" />
        <view class="contact-info">
          <h4>{{ contact.name }}</h4>
          <p>{{ contact.lastMessage }}</p>
        </view>
      </view>
    </view>

    <!-- 右侧聊天窗口 -->
    <view class="chat-window">
      <view class="chat-header">
        <img :src="currentContact.avatar" alt="头像" class="avatar" />
        <h4>{{ currentContact.name }}</h4>
      </view>

      <view class="chat-content" ref="chatContent">
        <view
          v-for="message in currentContact.messages"
          :key="message.id"
          :class="['chat-message', message.sender === 'me' ? 'sent' : 'received']"
        >
          <p>{{ message.text }}</p>
          <span class="message-time">{{ message.time }}</span>
        </view>
      </view>

      <view class="chat-input">
        <input
          type="text"
          v-model="newMessage"
          placeholder="输入消息..."
          @keyup.enter="sendMessage"
        />
        <button @click="sendMessage">发送</button>
      </view>
    </view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      contacts: [
        {
          id: 1,
          name: '张栋',
          avatar: '/static/toux13.jpg',
          lastMessage: 'Hey, how are you?',
          messages: [
            { id: 1, text: '你们的项目完成的怎么样了?', sender: '张栋', time: '10:01 AM' },
            { id: 2, text: '哦!my dear tutor~实在是太抱歉了！完成进度堪忧啊我们', sender: 'me', time: '10:02 AM' }
          ]
        },
        {
          id: 3,
          name: '张诗雨',
          avatar: '/static/toux2.jpg',
          lastMessage: 'What’s up?',
          messages: [
			{ id: 3, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '11:30 AM' },
			{ id: 2, text: '再玩会', sender: '张诗雨', time: '11:32 AM' },
          ]
        }
      ],
      currentContact: null, // 当前选中的联系人
      newMessage: '' // 新消息输入框
    };
  },
  methods: {
    // 选择联系人进行聊天
    selectContact(contact) {
      this.currentContact = contact;
      this.$nextTick(() => {
        // 自动滚动到最新消息
        this.scrollToBottom();
      });
    },
    // 发送消息
    sendMessage() {
      if (this.newMessage.trim() === '') return; // 如果输入为空，直接返回

      const message = {
        id: Date.now(),
        text: this.newMessage,
        sender: 'me',
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      };

      // 将新消息加入当前联系人的消息列表
      this.currentContact.messages.push(message);
      this.newMessage = ''; // 清空输入框

      this.$nextTick(() => {
        // 发送完消息后自动滚动到底部
        this.scrollToBottom();
      });
    },
    // 滚动到最新消息
    scrollToBottom() {
      const chatContent = this.$refs.chatContent;
      chatContent.scrollTop = chatContent.scrollHeight;
    }
  },
  mounted() {
    // 默认选择第一个联系人
    this.currentContact = this.contacts[0];
  }
};
</script>
<style scoped>
.chat-app {
  display: flex;
  height: 100vh;
  background-color: #f0f0f0;
}

.contact-list {
  width: 25%;
  background-color: #fff;
  border-right: 1px solid #ddd;
  overflow-y: auto;
}

.contact-item {
  padding: 15px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.contact-item.active {
  background-color: #f0f8ff;
}

.contact-item:hover {
  background-color: #e6f7ff;
}

.avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  margin-right: 15px;
}

.contact-info h4 {
  margin: 0;
  font-size: 16px;
}

.contact-info p {
  margin: 5px 0 0;
  font-size: 12px;
  color: #888;
}

.chat-window {
  width: 75%;
  display: flex;
  flex-direction: column;
}

.chat-header {
  background-color: #fff;
  padding: 20px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ddd;
}

.chat-header h4 {
  margin-left: 15px;
}

.chat-content {
  flex-grow: 1;
  padding: 20px;
  overflow-y: auto;
  background-color: #f9f9f9;
}

.chat-message {
  margin-bottom: 10px;
  max-width: 60%;
  padding: 10px;
  border-radius: 8px;
  position: relative;
}

.sent {
  background-color: #007bff;
  color: white;
  align-self: flex-end;
}

.received {
  background-color: #eee;
  align-self: flex-start;
}

.message-time {
  font-size: 10px;
  color: #999;
  margin-top: 5px;
  display: block;
  text-align: right;
}

.chat-input {
  display: flex;
  padding: 10px;
  background-color: #fff;
  border-top: 1px solid #ddd;
}

.chat-input input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 20px;
  margin-right: 10px;
}

.chat-input button {
  padding: 10px 20px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 20px;
  cursor: pointer;
}

.chat-input button:hover {
  background-color: #0056b3;
}
</style>
