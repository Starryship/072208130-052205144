<template>
  <view>
    <!-- 表单输入区域 -->
    <form @submit.prevent="addProject">
      <view>
        <label for="projectName" style="width: 95%; height: 10%; padding: 10px;">项目名称：</label>
        <input type="text" v-model="newProject.name" id="projectName" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectDomain" style="width: 95%; height: 10%; padding: 10px;">项目领域：</label>
        <input type="text" v-model="newProject.domain" id="projectDomain" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectTalent" style="width: 95%; height: 10%; padding: 10px;">所需技能：</label>
        <input type="text" v-model="newProject.requiredTalent" id="projectTalent" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectMentor" style="width: 95%; height: 10%; padding: 10px;">指导老师：</label>
        <input type="text" v-model="newProject.mentor" id="projectMentor" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectTime" style="width: 95%; height: 10%; padding: 10px;">项目时间：</label>
        <input type="text" v-model="newProject.time" id="projectTime" placeholder="如：2023.10.12-2025.10.12" style="width: 95%; height: 10%; padding: 10px;"/>
      </view>
      <view>
        <label for="projectIntro" style="width: 95%; height: 10%; padding: 10px;">项目简介：</label>
        <textarea v-model="newProject.inroduction" id="projectIntro" style="padding: 10px;"></textarea>
      </view>
      <view>
        <label for="projectTarget" style="width: 95%; height: 10%; padding: 10px;">项目目标：</label>
        <input type="text" v-model="newProject.target" id="projectTarget" class="inputGray" style="width: 95%; height: 10%; padding: 10px;"/>
		
      </view>
      <view>
        <label for="projectMembers" style="width: 95%; height: 10%; padding: 10px;">队员（用逗号分隔）：</label>
        <input type="text" style="width: 95%; height: 10%; padding: 10px;" v-model="newProject.memberInput" id="projectMembers" placeholder="如：Bob, Alice, Chris" />
      </view>
      <view>
        <label for="projectTasks" style="width: 95%; height: 10%; padding: 10px;">任务列表（用逗号分隔）：</label>
        <input type="text" v-model="newProject.taskListInput" id="projectTasks" placeholder="如：原型设计, 编程实现" style="width: 95%; height: 10%; padding: 10px;" />
      </view>
	  
	  <br/>
      <button type="submit" class="add-button" @click="addProject">创建项目</button>
    </form>

    <!-- 显示项目列表 -->
<!--   <view class="project-list">
      <h2>项目列表</h2>
      <view v-for="(project, index) in projectList" :key="project.id" class="project-item">
        <h3>{{ index + 1 }}. {{ project.name }}</h3>
        <p>领域：{{ project.domain }}</p>
        <p>所需技能：{{ project.requiredTalent }}</p>
        <p>指导老师：{{ project.mentor }}</p>
        <p>项目时间：{{ project.time }}</p>
        <p>简介：{{ project.inroduction }}</p>
        <p>目标：{{ project.target }}</p>
        <p>队员：{{ project.member.join(", ") }}</p>
        <p>任务列表：{{ project.taskList.join(", ") }}</p>
      </view>
    </view> -->
	
	
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
/* 样式定义 */
form {
  margin: 30px;

}

form view {
  margin-bottom: 20px;
  left: 10%;
    width: 90%;
	height: 10%;
}

label {
  display: flex;
  width: 100px;
  height: 10%;
  
  left:30px;
}



.add-button {
  background-color: #1e90ff;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #4682b4;
}

.project-list {
  margin-top: 20px;
}

.project-item {
  border: 1px solid #ddd;
  padding: 10px;
  margin-bottom: 10px;
}


input[type=text] {
  width: 80%;
  height: 10px;
  
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 2px solid #ccc;
  border-radius: 7px;
  box-sizing: border-box;
  

  
}

/* .inputGray{
	height:20%;
} */


button[type=submit] {
	
  width: 80%;
  height: 50px;
  
  background-color: #4CAF50;
  color: white;
  padding-bottom: 10px;
  margin: 8px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
left: 30px;
bottom:5px;

  text-align: center; /* 水平居中 */
  line-height: 30px; /* 设置行高与按钮高度相等，垂直居中 */
  font-size: 20px; /* 文字大小 */


  
}

button[type=submit]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}


</style>
