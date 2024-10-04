<template>
  <view id="app">
    <view class="container">
      <h2>伙伴资料</h2>
      
      <!-- Profile Section -->
      <view class="profile-section">
        <label>姓名</label>
        <input v-model="formData.name" placeholder="Name" />

        <view class="profile-picture">
          <img :src="formData.imageUrl" alt="profile" />
        </view>
      </view>

      <!-- Fields Section -->
      <view class="field">
        <label>专业</label>
        <input v-model="formData.major" placeholder="Major" />
      </view>
      <view class="field">
        <label>学号</label>
        <input v-model="formData.studentId" placeholder="Student ID" />
      </view>
      <view class="field">
        <label>联系方式</label>
        <input v-model="formData.email" placeholder="email" />
        <input v-model="formData.phone" placeholder="phone" />
      </view>

      <!-- Interests Section -->
      <view class="field">
        <label>感兴趣的方向</label>
        <textarea v-model="formData.interests" placeholder="Interests"></textarea>
      </view>

      <!-- Projects Section -->
      <view class="field">
        <label>参与的项目展示</label>
        <textarea v-model="formData.projects" placeholder="Projects"></textarea>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: 'Bob',
        major: '',
        studentId: '',
        email: '',
        phone: '',
        interests: '',
        projects: '',
        imageUrl: '/static/toux2.jpg', // Placeholder profile image URL
      },
    };
  },
  methods: {
    goTo(page) {
      // Navigation logic here
      console.log(`Navigating to ${page}`);
    },
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

.profile-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.profile-picture img {
  width: 60px;
  height: 60px;
  margin: 20px;
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
  height: 82px;
}
</style>

