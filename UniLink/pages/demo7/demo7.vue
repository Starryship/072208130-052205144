<template> 
  <view class="header">
  	<h2>
		<img src="/static/toux3.jpg" class="tou" @click="person()" />
		<span class="text">项目拍档</span>
		<!-- <span class="add">+</span> -->
		<view class="add-partner-popup" v-if="visible">  
		    <view class="popup-overlay" @click="closePopup"></view>  
		    <view class="popup-content" @click.stop>  
		      <button @click="addPartner">添加拍档</button>  
		      <button @click="scanQRCode">扫一扫</button>  
		      <button @click="closePopup">取消</button>  
		    </view>  
		  </view>  
		  <view class="add-partner-button" @click="openPopup">+</view> 
		</h2>
  </view>
  <hr class="hr"/>
  <view class="contact-selector" style="padding-left: 1px;">  
    <ul>  
      <li v-for="contact in contacts" :key="contact.id" @click="selectContact(contact)">  
        <img :src="contact.avatar" alt="Contact Avatar" class="avatar" />  
        <span class="name">{{ contact.name }}</span>  
        <span class="status">{{ contact.status }}</span>  
      </li>  
    </ul>  
  </view>  
</template>  
  
<script>  
export default {  
  data() {  
    return {  
      contacts: [  
		{ id: 0, name: '共谋大事组', avatar: 'static/toux1.jpg', status: '' },  
        { id: 1, name: 'tutor:张栋', avatar: 'static/toux13.jpg', status: 'Online' },  
        { id: 2, name: '曹星辰', avatar: 'static/toux3.jpg', status: 'Online' },  
        { id: 3, name: '张诗雨', avatar: 'static/toux2.jpg', status: 'Online' }, 
		{ id: 4, name: '詹镇號', avatar: 'static/toux10.jpg', status: 'Online' }, 
		{ id: 5, name: '刘哲锐', avatar: 'static/toux12.jpg', status: 'Away' }, 
		{ id: 6, name: '黄悦迦', avatar: 'static/toux7.jpg', status: 'Away' }, 
		{ id: 7, name: '朱佳捷', avatar: 'static/toux14.jpg', status: 'Away' }, 
		{ id: 8, name: '吴三丰', avatar: 'static/toux8.jpg', status: 'Offline' }, 
		{ id: 9, name: '严加一', avatar: 'static/toux6.jpg', status: 'Offline' }, 
		{ id: 10, name: '黄馨贻', avatar: 'static/toux15.jpg', status: 'Offline' }, 
		{ id: 11, name: '韦❤茹', avatar: 'static/toux4.jpg', status: 'Offline' }, 
		{ id: 12, name: '郑❤艺', avatar: 'static/toux5.jpg', status: 'Offline' }, 
        // 可以根据需要添加更多联系人  
      ],  
      selectedContact: null,  
	  visible: false,
    };  
  },  
  methods: {
	person(){
		uni.switchTab({
			url:"/pages/demo10/demo10"
		})
	},
    selectContact(contact) {  
      this.selectedContact = contact;  
      // 这里可以添加跳转到聊天页面的代码，例如使用vue-router  
      console.log('Selected contact:', contact);  
      // 假设你有一个方法或路由可以导航到聊天页面  
      // this.$router.push({ name: 'Chat', params: { contactId: contact.id } });  
	  // this.$router.push({ name: 'ContactDetail', params: { id: contact.id, contact: contact } });
    },  
	openPopup() {  
	      this.visible = true;  
	    },  
	    closePopup() {  
	      this.visible = false;  
	    },  
	    addPartner() {  
	      // 实现添加拍档的逻辑  
	      alert('添加拍档');  
	      this.closePopup();  
	    },  
	    scanQRCode() {  
	      // 实现扫一扫的逻辑  
	      // 注意：这里需要集成扫码库并处理摄像头权限等  
	      alert('扫一扫');  
	      this.closePopup();  
	    }
  },  
};  
</script>  
  
<style scoped>  
.contact-selector {  
  width: 300px;  
  max-width: 100%;  
  margin: 0 auto;  
  padding: 20px; 
  border-radius: 8px;  
  text-align: center;  
  background-color:#fff;
}  
.hr{
	margin-top:8px;
}
h2{
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-bottom: 0px;
}
.header{
	margin-top: 3px;
}
.tou{
	margin-top: 10px;
	margin-left: 30px;
	width: 50px;
	height: 50px;  
	border-radius: 50%;  
	margin-right: 15px; 
}
.add{
  margin-right: 30px;
  font-size: 30px;
} 
.text{
	margin-right: 30px;
	margin-top: 5px;
	font-size: 18px;
}
ul {  
  list-style-type: none;  
  padding: 0;  
}  
  
li {  
  width: 100%;
  display: flex;  
  align-items: center;  
  margin-bottom: 15px;  
  padding: 10px;  
  border: 1px solid #eee;  
  border-radius: 8px;  
  background-color: rgb(42,173,255,0.2);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);  
  transition: background-color 0.3s;
}  
  
li:hover {  
  background-color: #f0f8ff;  
}  
  
.avatar {  
  width: 50px;  
  height: 50px;  
  border-radius: 50%;  
  margin-right: 15px; 
}  
  
.name {  
  font-size: 16px;  
  font-weight: bold;  
}  
  
.status {  
  font-size: 12px;  
  color: #888;  
}  
.add-partner-popup {  
  position: fixed;  
  top: 0;  
  left: 0;  
  right: 0;  
  bottom: 0;  
  display: flex;  
  justify-content: center;  
  align-items: center;  
  z-index: 1000;  
}  
  
.popup-overlay {  
  position: absolute;  
  top: 0;  
  left: 0;  
  right: 0;  
  bottom: 0;  
  background-color: rgba(0, 0, 0, 0.1);  
}  
  
.popup-content {  
  width: 200px;
  background-color: white;  
  padding: 0;  
  border-radius: 5px;  
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);  
  text-align: center;  
}  
  
.add-partner-button {  
  width: 50px;  
  height: 50px;   
  color: #000;  
  text-align: center;  
  line-height: 50px;  
  font-size: 24px;  
  cursor: pointer;  
  border-radius: 50%;  
}  
</style>