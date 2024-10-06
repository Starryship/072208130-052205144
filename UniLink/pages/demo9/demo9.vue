<template>
	<view class="chat-app">
		<view class="header">
			<h2>伙伴资料</h2>
		</view>
		<!-- 右侧详细信息表单 -->
		<view class="contact-info" v-if="currentContact">
			<img :src="currentContact.avatar" alt="头像" class="large-avatar" />
			<view class="info-item">
        <label class="name">姓名:</label>
        <input v-model="currentContact.name" disabled />
      </view>
	</view>  
	<view class="info-item">
        <label>专业:</label>
        <input v-model="currentContact.major" disabled />
    </view>
    <view class="info-item">
        <label>学号:</label>
        <input v-model="currentContact.Id" disabled />
    </view>
	<label class="contact">联系方式:</label>
    <view class="info-item contact-info">
        
        <input v-model="currentContact.contact.email" disabled />
        <input v-model="currentContact.contact.phone" disabled />
    </view>
    <view class="info-item">
        <label>感兴趣的方向:</label>
        <textarea v-model="currentContact.interests" disabled></textarea>
    </view>
    <view class="info-item">
        <label>参与的项目展示:</label>
        <textarea v-model="currentContact.projects" disabled></textarea>
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
		      major: '软件工程',
		      Id: 'NULL',
		      contact: {
		        email: '',
		        phone: ''
		      },
		      interests: '',
		      projects: '跨专业合作APP',
		      avatar: '/static/toux1.jpg'
		    },
            {
              id: 1,
              name: '张栋',
              major: '软件工程',
              Id: '0000001',
              contact: {
                email: 'zhangdong@edu.fzu.cn',
                phone: '15111707151'
              },
              interests: '足球篮球乒乓球',
              projects: '跨专业合作APP',
              avatar: '/static/toux13.jpg'
            },
            {
              id: 2,
              name: '张诗雨',
              major: '数据科学与大数据技术',
              Id: '052205144',
              contact: {
                email: '052205144@edu.fzu.cn',
                phone: '18554863205'
              },
              interests: '王者荣耀',
              projects: '跨专业合作APP',
              avatar: '/static/toux2.jpg'
            },
			{
			  id: 3,
			  name: '詹镇號',
			  major: '数据科学与大数据技术',
			  Id: '102202149',
			  contact: {
			    email: '102202149@edu.fzu.cn',
			    phone: '1845863406'
			  },
			  interests: '螺蛳粉',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux10.jpg'
			},
			{
			  id: 4,
			  name: '刘哲锐',
			  major: '数据科学与大数据技术',
			  Id: '102202111',
			  contact: {
			    email: '102202111@edu.fzu.cn',
			    phone: '18456853107'
			  },
			  interests: '瓦罗兰特',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux12.jpg'
			},
			{
			  id: 5,
			  name: '黄悦迦',
			  major: '数据科学与大数据技术',
			  Id: '102202142',
			  contact: {
			    email: '102202142@edu.fzu.cn',
			    phone: '18486345461'
			  },
			  interests: '瓦罗兰特',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux7.jpg'
			},
			{
			  id: 6,
			  name: '朱佳捷',
			  major: '数据科学与大数据技术',
			  Id: '012201201',
			  contact: {
			    email: '012201201@edu.fzu.cn',
			    phone: '15258123692'
			  },
			  interests: '打游戏',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux14.jpg'
			},
			{
			  id: 7,
			  name: '吴三丰',
			  major: '智能电网',
			  Id: '052205136',
			  contact: {
			    email: '052205136@edu.fzu.cn',
			    phone: '15054691523'
			  },
			  interests: '第五人格',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux8.jpg'
			},
			{
			  id: 8,
			  name: '严加一',
			  major: '工程管理',
			  studentId: '052205135',
			  contact: {
			    email: '052205135@edu.fzu.cn',
			    phone: '15051654805'
			  },
			  interests: '抖音',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux6.jpg'
			},
			{
			  id: 9,
			  name: '黄馨贻',
			  major: '数据科学与大数据技术',
			  studentId: '102202142',
			  contact: {
			    email: '102202142@edu.fzu.cn',
			    phone: '15489632805'
			  },
			  interests: '学习',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux15.jpg'
			},
			{
			  id: 10,
			  name: '韦❤茹',
			  major: '工程管理',
			  studentId: '052205143',
			  contact: {
			    email: '052205143@edu.fzu.cn',
			    phone: '18654781256'
			  },
			  interests: '吃鸡',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux4.jpg'
			},
			{
			  id: 11,
			  name: '郑❤艺',
			  major: '会计学',
			  studentId: '052205135',
			  contact: {
			    email: '052205135@edu.fzu.cn',
			    phone: '15164852374'
			  },
			  interests: '数钱',
			  projects: '跨专业合作APP',
			  avatar: '/static/toux5.jpg'
			},
          ],
 		  currentContact: null, 
		  ID: null,
       };
  },
  mounted() {
	  // 默认选择第一个联系人
	const contactId = this.$route.query.id; // 获取 URL 中的 id 参数
	console.log(contactId);
	this.ID=contactId;
	console.log(this.ID);
	this.currentContact = this.contacts1[this.ID];
  },
};
</script>

<style scoped>
template {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.chat-app{
  padding: 20px;
  background-color: rgb(209,243,255,0.5);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  margin: 15px;
}

h2 {
  text-align: center;
  color: #333;
}

.contact-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.large-avatar{
  width: 60px;
  height: 60px;
  border-radius: 50%;
}

.info-item {
  margin-bottom: 15px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input, textarea {
  width: 90%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 5px;
  background-color: #fff;
}
.name{
	width: 82vw;
}
textarea {
  resize: none;
  height: 82px;
}
.contact-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.contact{
	margin-left:8px;
}
</style>
