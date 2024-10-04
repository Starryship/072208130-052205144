<template>  
  <view class="settings-page">  
    <h1>设置</h1>  
    <view class="options">  
      <view class="option" v-for="(option, index) in options" :key="index">  
        <label>  
          <input type="checkbox" :value="option.value" v-model="selectedOptions" />  
          {{ option.label }}  
        </label>  
      </view>  
    </view>  
	
	<!-- 展开更多内容 -->
    <view class="more-content">
      <span @click="toggleMoreContent">点击展开更多内容</span>
      <view v-if="showMoreContent" class="extra-content">
        <p>啊哦，没有更多内容了...</p>
      </view>
    </view>
	
	<view class="buttons">
		<button @click="saveSettings"><b>保存设置</b></button>  
	</view>
    <view class="buttons">  
      <button @click="logout"><b>退出登录</b></button>  
    </view>  
	
  </view>  
</template>  
  
<script>  
export default {  
  data() {  
    return {  
      options: [  
        { label: '项目一：是否展示在个人资料页', value: 'option1' },  
        { label: '项目二：是否展示在个人资料页', value: 'option2' },  
        { label: '项目三：是否展示在个人资料页', value: 'option3' },  
      ],  
      selectedOptions: [],  
	  showMoreContent: false,
    };  
  },  
  methods: { 
	toggleMoreContent() {
	      this.showMoreContent = !this.showMoreContent;
	    },
    logout() {  
      // 实现退出登录的逻辑  
      alert('已退出!');  
	  uni.navigateTo({
	  	url: "/pages/demo1/demo1"
	  })
    },  
    saveSettings() {  
      // 实现保存设置的逻辑  
      alert('已保存设置: ' + this.selectedOptions.join(', '));  
	  uni.switchTab({
	  	url: "/pages/demo10/demo10"
	  })
    },  
  },  
};  
</script>  
  
<style scoped>  
.settings-page {  
  max-width: 300px;  
  margin: 20px auto;  
  padding: 20px;  
  background-color: rgb(42,173,255,0.2);  
  border-radius: 8px;  
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);  
  height:600px
}  
  
h1 {  
  text-align: center;  
  color: #333;  
  margin: 20px;
  margin-bottom: 30px;
}  
  
.options {  
  display: flex;  
  
  flex-direction: column;  
  gap: 30px;  
}  
  
.option {  
  background-color: #fff;  
  padding: 10px;  
  border: 1px solid #ccc;  
  border-radius: 10px;  
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); 
}  
  
.option label {  
  display: flex;  
  align-items: center;  
  gap: 10px;  
}  
  
.buttons {  
  display: flex;  
  justify-content: space-between;  
}  
  
button {  
  margin-top: 40px; 
  padding: 10px 20px;  
  background-color: rgb(42,173,255,0.5);
  font-size: 20px;
  color: #fff;  
  border: none;  
  border-radius: 20px;  
  cursor: pointer; 
  display: block;
  width: 90%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.8);
}  
  
button:hover {  
  background-color: #0056b3;  
}  
.more-content {
  margin-top:20px ;
  margin-bottom: 20px;
  color: #007BFF;
  cursor: pointer;
}
.extra-content {
  margin-top: 10px;
  font-size: 14px;
  color: #555;
}
</style>