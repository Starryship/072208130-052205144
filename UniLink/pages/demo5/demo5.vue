<template>
  <view>
    <!-- 表单输入区域 -->
    <form @submit.prevent="addProject">
      <view>
        <label for="projectName" style="width: 95%; height: 10%; padding: 10px;">项目名称：</label>
        <input type="text" class="inputGray" v-model="newProject.name" id="projectName" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectDomain" style="width: 95%; height: 10%; padding: 10px;">项目领域：</label>
        <input type="text" class="inputGray" v-model="newProject.domain" id="projectDomain" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectTalent" style="width: 95%; height: 10%; padding: 10px;">所需技能：</label>
        <input type="text" class="inputGray" v-model="newProject.requiredTalent" id="projectTalent" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectMentor" style="width: 95%; height: 10%; padding: 10px;">指导老师：</label>
        <input type="text" class="inputGray" v-model="newProject.mentor" id="projectMentor" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectTime" style="width: 95%; height: 10%; padding: 10px;">项目时间：</label>
        <input type="text" class="inputGray" v-model="newProject.time" id="projectTime" placeholder="如：2023.10.12-2025.10.12" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectIntro" style="width: 95%; height: 10%; padding: 10px;">项目简介：</label>
        <textarea class="inputGray1" v-model="newProject.inroduction" id="projectIntro" style="padding: 10px;"></textarea>
      </view>
      <view>
        <label for="projectTarget" style="width: 95%; height: 10%; padding: 10px;">项目目标：</label>
        <input type="text" v-model="newProject.target" id="projectTarget" class="inputGray" style="width: 95%; height: 10%; padding: 10px;"/>
		
      </view>
      <view>
        <label for="projectMembers" style="width: 95%; height: 10%; padding: 10px;">队员（用逗号分隔）：</label>
        <input type="text" class="inputGray" style="width: 95%; height: 10%; padding: 10px;" v-model="newProject.memberInput" id="projectMembers" placeholder="如：Bob, Alice, Chris" />
      </view>
      <view>
        <label for="projectTasks" style="width: 95%; height: 10%; padding: 10px;">任务列表（用逗号分隔）：</label>
        <input type="text" class="inputGray" v-model="newProject.taskListInput" id="projectTasks" placeholder="如：原型设计, 编程实现" style="width: 95%; height: 10%; padding: 10px;" />
      </view>
	  
	  <br/>
      <button type="submit" class="add-button" @click="addProject">创建项目</button>
    </form>
  </view>
</template>

<script>
import { ref ,reactive} from "vue";
// import { useRouter } from 'vue-router';


export default {
  setup() {
    // 初始化项目列表
    const projectList = ref([]);

    // 新项目数据模板
    const newProject = reactive({
      name: "",
      domain: "",
      requiredTalent: "",
      mentor: "",
      time: "",
      inroduction: "",
      target: "",
      memberInput: "",
      taskListInput: "",
    });



    // 添加新项目到项目列表
    const addProject = () => {
		// console.log('addProject')
      const id = projectList.value.length + 1; // 自动生成新项目的 ID
	  // console.log(newProject.value)
      const members = newProject.memberInput.split(",").map((item) => item.trim());
      const tasks = newProject.taskListInput.split(",").map((item) => item.trim());

      // 构建新项目对象
      const project = {
        id,
        name: newProject.name,
        domain: newProject.domain,
        requiredTalent: newProject.requiredTalent,
        mentor: newProject.mentor,
        time: newProject.time,
        inroduction: newProject.inroduction,
        target: newProject.target,
        member: members,
        taskList: tasks,
      };

      // 添加到项目列表
      projectList.value.push(project);

      // 清空表单
      // 清空表单
      for (const key in newProject) {
        newProject[key] = ""; // 重置每个字段的值
      }

      console.log("新项目添加成功：", project);
      console.log("当前项目列表：", projectList.value);
	  
	  
	  
	  
	  
	  
	  const projectData = JSON.stringify(project);
	  // 通过路由传递数据
	  
	  console.log(projectData)

	  
	  
	  uni.setStorage({
	    key: 'projectData',
	    data: projectData,
	    success: function() {
	      uni.switchTab({
	        url: '/pages/demo4/demo4'
	      });
	    }
	  });
	  
	  
	  
	  
    };


	
	
    const submitForm = () => {
      // 将 projectList 转换为 JSON 字符串
      const projectData = JSON.stringify(newProject.value);
      // 通过路由传递数据
	  
	  console.log(projectData)
	  
      uni.switchTab({
        url: `/pages/demo4/demo4?projects=${encodeURIComponent(projectData)}`
      });
    };



    return {
      projectList,
      newProject,
      addProject,
	  submitForm,
    };
  },
};
</script>



<style scoped>
/* 表单样式 */
form {
  margin: 5px;
  margin-right: 25px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.label {
  font-size: 16px;
  font-weight: bold;
  margin: 5px 0;
  color: #333;
}

.input-field, .textarea-field {
  width: 95%;
  padding: 12px;
  border: 2px solid #ccc;
  border-radius: 10px;
  margin-bottom: 15px;
  box-sizing: border-box;
  background-color: #F;
  font-size: 14px;
  color: #333;
}

.textarea-field {
  height: 100px;
  resize: none;
}

/* 提交按钮样式 */
.add-button {
  width: 95%;
  background-color: #1e90ff;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 18px;
  text-align: center;
}

.add-button:hover {
  background-color: #4682b4;
}

.add-button:active {
  background-color: #3c8dbc;
  transform: scale(0.98);
}
.inputGray{
	width: 95%;
	padding: 12px;
	border: 2px solid #ccc;
	border-radius: 10px;
	margin-bottom: 15px;
	box-sizing: border-box;
	background-color: #FFF;
	font-size: 14px;
	color: #333;
}
.inputGray1{
	/* width: 100%; */
	padding: 12px;
	border: 2px solid #ccc;
	border-radius: 10px;
	margin-bottom: 15px;
	box-sizing: border-box;
	background-color: #FFF;
	font-size: 14px;
	color: #333;
}
/* 其他样式 */
view {
  width: 100%;
  padding: 10px;
}

body {
  background-color: #f0f4f7;
}
</style>
