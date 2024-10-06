<template>
  <view id="app">  
    <view class="container">
      <!-- Header Section with Title and Edit Icon -->
      <view class="header">
        <h2>个人资料</h2>
      </view>
	  <view class="">
		<img src="/static/Edit3.png" class="icon edit-icon"></img>
		<span @click="toggleInput">{{ isInputDisabled ? '编辑' : '提交' }}</span>
	  </view>
      <!-- Profile Section -->
      <view class="profile-section">
        <view>
          <label>姓名</label>
          <input :disabled="isInputDisabled" v-model="formData.name" placeholder="Name" />
        </view>
        <view class="profile-picture">
          <img :src="src" alt="profile" class="avatar" @tap="upload" />
		  <button class="btn">更换头像</button>
        </view>
      </view>

      <!-- Fields Section -->
      <view class="field">
        <label>专业</label>
        <input :disabled="isInputDisabled" v-model="formData.major" placeholder="Major" />
      </view>
      <view class="field">
        <label>学号</label>
        <input :disabled="isInputDisabled" v-model="formData.studentId" placeholder="Student ID" />
      </view>

      <!-- Contact Info -->
      <view class="field contact-info">
        <label>联系方式</label>
        <input :disabled="isInputDisabled" v-model="formData.email" placeholder="email" />
        <input :disabled="isInputDisabled" v-model="formData.phone" placeholder="phone" />
        <!-- Privacy Settings Icon -->
        <view class="privacy-settings">
          <i class="icon settings-icon"></i>
          <span @tap="pr_edit()">隐私设置</span>
        </view>
      </view>

      <!-- Interests Section -->
      <view class="field">
        <label>感兴趣的方向</label>
        <textarea :disabled="isInputDisabled" v-model="formData.interests" placeholder="Interests"></textarea>
      </view>

      <!-- Projects Section -->
      <view class="field">
        <label>参与的项目展示</label>
        <textarea :disabled="isInputDisabled" v-model="formData.projects" placeholder="Projects"></textarea>
      </view>
    </view>
  </view>
</template>
<script>
	export default {
	  data: {
		  src: ''
	  },
	  data() {
		return {
			src: 'static/toux3.jpg',
			isInputDisabled: true,
		  formData: {
			name: '曹星辰',
			major: '数据科学与大数据技术',
			studentId: '072208130',
			email: '072208130@edu.fzu.cn',
			phone: '18175412525',
			interests: '死读书，读死书',
			projects: '跨专业合作APP',
			imageUrl: 'static/toux3.jpg', // Updated profile image
		  },
		};
	  },
	  methods: {
		  upload() {
		  	uni.chooseImage({
		  		count: 1, // 默认9
		  		sizeType: ['compressed'], // 可以指定是原图还是压缩图，默认二者都有
		  		sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
		  		success(res) {
		  			const src = res.tempFilePaths[0];
					console.log(src);
		  			uni.redirectTo({
		  				url: '../upload/upload?src=' + src 
		  			});
		  		}
		  	});
		  },
		//隐私设置
		pr_edit() {
			uni.navigateTo({url: "/pages/demo11/demo11"})
		},
		toggleInput() {
		      // 切换输入框的禁用状态
		      this.isInputDisabled = !this.isInputDisabled;
		      
		      // 如果输入框变为可编辑，可以考虑清空内容或者做其他处理
		      if (!this.isInputDisabled) {
		        this.newMessage = ''; // 例如清空输入框
		      } else {
		        // 提交输入的消息
		        this.sendMessage(); // 调用发送消息的方法
		      }
		    },
	  },
	  onLoad(option) {
	  	let { avatar } = option;
	  	if (avatar) {
	  		this.src=avatar;
	  	}
	  }
	};
</script>
<style scoped>
template {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
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
.icon {
  font-size: 20px;
  cursor: pointer;
}

.profile-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.profile-picture img {
  width: 60px;
  height: 60px;
  border-radius: 50%;
}

.field {
  margin-bottom: 15px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}
.avatar{
	margin-left: 8px;
}
input, textarea {
  width: 90%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 5px;
  background-color: #fff;
}

textarea {
  resize: none;
  height: 60px;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.privacy-settings {
  display: flex;
  align-items: center;
  color: #666;
  cursor: pointer;
  margin-top: 10px;
}

.settings-icon::before {
  content: "⚙️";
  font-size: 20px;
  margin-right: 5px;
}
.btn{
	height: 4vh;
	font-size: 13px;
	background-color: aliceblue;
}
</style>
