<template>
  <view class="chat-app">
    <!-- 左侧联系人列表 -->
    <view class="contact-list">
      <view
        v-for="contact1 in contacts1"
        :key="contact1.id"
        :class="['contact-item', { active: contact1.id === currentContact.id }]"
        @click="selectContact(contact1)"
      >
        <img :src="contact1.avatar" alt="头像" class="avatar" />
      </view>
    </view>

    <!-- 右侧聊天窗口 -->
    <view class="chat-window">
      <view class="chat-header">
        <img :src="currentContact.avatar" alt="头像" class="avatar" @click="nav()"/>
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
        <button @click="sendMessage"><span class="send">发送</span></button>
      </view>
    </view>
  </view>
</template>
<script>
import {
	ref,
	onMounted,
	getCurrentInstance,
	computed
} from 'vue'; // 引入 onMounted 和 getCurrentInstance
export default {
  data() {
    return {
      contacts1: [
		{
		  id: 0,
		  name: '共谋大事组',
		  avatar: '/static/toux1.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [],
		},
        {
          id: 1,
          name: '张栋',
          avatar: '/static/toux1.jpg',
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
			{ id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '11:30 AM' },
			{ id: 3, text: '还没睡醒', sender: '张诗雨', time: '11:32 AM' },
          ]
        },
		{
		  id: 4,
		  name: '詹镇號',
		  avatar: '/static/toux10.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 4, text: '还在睡', sender: '詹镇號', time: '10:02 AM' }
		  ]
		},
		{
		  id: 5,
		  name: '刘哲锐',
		  avatar: '/static/toux12.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 5, text: 'wa', sender: '刘哲锐', time: '10:02 AM' }
		  ]
		},
		{
		  id: 6,
		  name: '黄悦迦',
		  avatar: '/static/toux7.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 6, text: '瓦', sender: '黄悦迦', time: '10:02 AM' }
		  ]
		},
		{
		  id: 7,
		  name: '朱佳捷',
		  avatar: '/static/toux14.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 7, text: '瓦？', sender: '朱佳捷', time: '10:02 AM' }
		  ]
		},
		{
		  id: 8,
		  name: '吴三丰',
		  avatar: '/static/toux8.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 8, text: '瓦罗兰特', sender: '吴三丰', time: '10:02 AM' }
		  ]
		},
		{
		  id: 9,
		  name: '严加一',
		  avatar: '/static/toux6.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 2, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 9, text: '启动！！', sender: '严加一', time: '10:02 AM' }
		  ]
		},
      ],
      currentContact: null, // 当前选中的联系人
      newMessage: '' ,// 新消息输入框
	  ID: null,
    };
  },
  methods: {
	nav(){
		uni.navigateTo({
			url:"/pages/demo9/demo9"
		})
	},
    // 选择联系人进行聊天
    selectContact(contact1) {
      this.currentContact = contact1;
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
	const contactId = this.$route.query.id; // 获取 URL 中的 id 参数
	console.log(contactId);
	this.ID=contactId;
	console.log(this.ID);
    this.currentContact = this.contacts1[this.ID];
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
  padding: 20px;
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
  width: 60px;
  height: 60px;
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
  height: 800px;
  display: flex;
  flex-direction: column;
}

.chat-header {
  background-color: #fff;
  padding: 20px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #000;
}

.chat-header h4 {
  margin-left: 15px;
}

.chat-content {
  flex-grow: 1;
  padding: 20px;
  overflow-y:auto;
  background-color: #f9f9f9;
}

.chat-message {
  margin-bottom: 25px;
  max-width: 80%;
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
  color: #000;
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
  font-size: 15px;
  width: 50px;
  height: 40px;
  padding: 2px 10px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 20px;
  cursor: pointer;
  white-space: nowrap;
}

.chat-input button:hover {
  background-color: #0056b3;
}
</style>
