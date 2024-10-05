<template>



	<view class="container-all" style="display: flex;flex-direction: column;padding-left: 3%;">
		<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper"
			@scrolltolower="lower" @scroll="scroll">



			<!-- 显示项目详情内容 -->
			<view v-if="project" class="center-container">
				<text class="project-name">{{ project.name }}</text>
				<view class="detail-white">
					<view class="content">
						<br />
						<span class="thick">领域：</span><text>{{ project.domain }}</text>
						<br />
						<span class="thick">项目简介：</span><text>{{ project.inroduction }}</text>
						<br />
						<span class="thick">项目目标：</span><text>{{ project.target }}</text>
						<br />
						<span class="thick">技能需求：</span><text>{{ project.requiredTalent }}</text>
						<br />
						<span class="thick">指导老师：</span><text>{{ project.mentor }}</text>
						<br />
						<span class="thick">已加入队员：</span><text v-for="(member, index) in project.member"
							:key="index">{{ member }}<span v-if="index < project.member.length - 1">, </span></text>
						<br />
						<span class="thick">时间：</span><text>{{ project.time }}</text>

					</view>
					<view class="task">
						<text class="content">任务列表</text>
						<br />
						<br />
						<checkbox-group class="uni-list" @change="checkboxChange" style="margin-left: 5%;">
							<label class="uni-list-cell uni-list-cell-pd" v-for="item in checkboxItems"
								:key="item.name">
								<view style="margin-top: 5%;">

									<checkbox :value="item.value" :checked="item.checked">{{ item.value }}</checkbox>
								</view>
								<!-- <view>{{ item.value }}</view> -->
							</label>
							<br>
						</checkbox-group>


					</view>


				</view>

			</view>



			<view v-if="project" class="center-container2">
				<view class="detail-white2">


					<view>
						<br>
						<text class="content">任务安排</text>

						<label class="uni-list-cell uni-list-cell-pd" v-for="item in checkboxItems" :key="item.name">
							<view style="margin-top: 5%;">

								<label
									style="width: 95%; height: 10%; padding: 10px;margin-bottom: 10px;">{{ item.value }}：</label>

								<input type="text" id="projectTime" placeholder="队员名字"
									style="width: 85%; height: 8%; padding: 10px;border: 2px solid #ccc;margin-top: 10px;" />
							</view>
							<!-- <view>{{ item.value }}</view> -->
						</label>

<br>

					</view>


				</view>
			</view>



			<view v-else>
				<text>未找到项目详情，请检查项目 ID 是否正确。</text>
			</view>

<button class="apply-buttom2" @click="handleClick">保存修改</button>
<button class="apply-buttom3">退出项目</button>

		</scroll-view>
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
		setup() {
			// 响应式数据，用于存储项目详细信息
			const project = ref(null);

			// 模拟的项目数据（可以从接口或数据库中获取）
			const projects = [{
					id: 1,
					name: '福uu',
					domain: '软件工程',
					requiredTalent: '软件工程师',
					mentor: '翁谦',
					time: "2023.10.12-2025.10.12",

					inroduction: "福uu是FZU校内app，用于提供课程信息等",
					target: "分析需求，设计app",
					member: ["Bob", "晨纸红", "朝兴财", "章师月"],
					taskList: ["原型设计", "编程实现"],
				},

				{
					id: 2,
					name: '联邦学习',
					domain: '分布式',
					requiredTalent: '大数据，机器学习',
					mentor: '郭坤',
					time: "2024.10.22-2025.05.12",

					inroduction: "一个机器学习框架，能有效帮助多个机构在满足用户隐私保护、数据安全和政府法规的要求下，进行数据使用和机器学习建模。",
					target: "优化算法",
					member: ["Bob"],
					taskList: ["横向联邦学习", "纵向联邦学习", "联邦迁移学习"],
				},
			];

			onMounted(() => {
				// 使用 getCurrentPages 获取当前页面栈
				const pages = getCurrentPages();
				const currentPage = pages[pages.length - 1]; // 获取当前页面实例

				// 从当前页面实例中获取路由参数 options
				const options = currentPage.options;
				const projectId = options.id; // 获取 id 参数


				// 模拟项目数据（实际中应从接口获取）


				// 查找对应 ID 的项目数据
				project.value = projects.find(p => p.id === Number(projectId)) || null;

				if (!project.value) {
					console.error(`未找到ID为 ${projectId} 的项目`);
				}
			});

			// 将 taskList 转换为 checkboxItems
			const checkboxItems = computed(() => {
				// 确保在访问 taskList 之前，project.value 不是 null
				if (project.value && project.value.taskList) {
					return project.value.taskList.map(task => {
						return {
							name: task,
							value: task,
							checked: false
						};
					});
				}
				return []; // 如果 project.value 为空，返回空数组
			});

			const checkboxChange = (event) => {
				console.log('选中的任务:', event.detail.value);
			};

			const handleClick = () => {
				//    alert('按钮被点击了');

				// 显示提示框
				uni.showToast({
					title: '修改已保存', // 提示内容
					icon: 'success', // 图标类型
					duration: 2000 // 持续时间，单位为毫秒
				});


			};



			// uni.getStorage({
			//   key: 'projectData',
			//   success: function(res) {
			//     if (res.data) {
			//       project.value.push(JSON.parse(res.data)); // 将数据解析并赋值给 projectList
			//     }
			//   },
			//   fail: function(err) {
			//     console.error('获取数据失败:', err);
			//   }
			// });




			return {
				project,
				checkboxItems,
				checkboxChange,
				handleClick,
			};
		},
	};
</script>

<style scoped>
	/* .project-detail {
  padding: 20px;
} */

	.project-name {
		display: flex;
		justify-content: center;
		font-size: 20px;
		font-weight: bold;
		color: #333;
		margin-bottom: 10px;

	}

	.project-detail {
		background-color: rgb(42, 173, 255, 0.1);
		padding: 20px;
		margin-bottom: 15px;
		border: 1px solid #e0e0e0;
		border-radius: 10px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

		justify-content: center;
		align-items: center;
		/* 垂直居中 */
		height: 50vh;


	}



	.thick {
		font-weight: bold;
		font-size: 15px;

	}

	/* .container-all {
		display: flex;
		justify-content: center;
		align-items: center;


		height: 100vh;

		width: 100vw;


	} */



	.detail-white {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		background-color: #ffffff;
		height: 50%;
		/* 设置容器高度为整个屏幕高度*/
		width: 90%;
		border-radius: 20px;
		/* padding-left: 10px; */

	}

	.content {
		padding: 15px;
		font-size: 15px;
		color: #888888;
		letter-spacing: 0.6px;
		line-height: 1.6
	}

	.task {
		margin: 20px;
		padding-top: 10px;
		border-radius: 20px;
		border: 2px solid #585858;
		height: 60%;
		/* 设置容器高度为整个屏幕高度*/
		width: 93%;
		margin-bottom: 10px;


	}



	.center-container {

		display: flex;
		flex-direction: column;
		justify-content: center;
		/* align-items: center; */

		height: 95%;
		/* 设置容器高度为整个屏幕高度*/
		width: 90%;
		border-radius: 20px;
		box-shadow: 5px 5px 20px 5px rgba(0, 0, 0, 0.2);
		background-color: rgb(178, 235, 255, 0.5);


		padding-left: 7%;

		padding-bottom: 10%;
		margin-top: 20%;
		margin-bottom: 20%;

	}

	.apply-buttom2 {

		font-size: 12px;

		width: 90px;
		/* 设置按钮的宽度 */
		height: 40px;

		position: absolute;
		/* 设置按钮为绝对定位 */
		top: 1%;
		/* 距离底部20px */
		right: 120px;
		/* 距离右边20px */
		/*background-color: #1e90ff;  按钮背景色 */
		color: black;
		/* 按钮字体颜色 */
		/*border: none;  去除边框 */
		border-radius: 10px;
		/*圆角 */
		padding: 5px;
		/* 按钮内边距 */
		cursor: pointer;
		/* 鼠标悬停效果 */
		box-shadow: 3px 3px 10px 0.1px rgba(0, 0, 0, 0.3);
		background-color: rgb(216, 245, 255);

	}
	
	
	.apply-buttom3 {
	
		font-size: 12px;
	
		width: 90px;
		/* 设置按钮的宽度 */
		height: 40px;
	
		position: absolute;
		/* 设置按钮为绝对定位 */
		top: 1%;
		/* 距离底部20px */
		right: 20px;
		/* 距离右边20px */
		/* background-color: #ff007f; */
		color: black;
		/* 按钮字体颜色 */
		/*border: none;  去除边框 */
		border-radius: 10px;
		/*圆角 */
		padding: 5px;
		/* 按钮内边距 */
		cursor: pointer;
		/* 鼠标悬停效果 */
		box-shadow: 3px 3px 10px 0.1px rgba(0, 0, 0, 0.3);
		background-color: rgba(231, 0, 0, 0.7);
	
	}
	
	
	.center-container2 {
	
		display: flex;
		flex-direction: column;
		justify-content: center;
		/* align-items: center; */
	
		height: 95%;
		/* 设置容器高度为整个屏幕高度*/
		width: 90%;
		border-radius: 20px;
		box-shadow: 5px 5px 20px 5px rgba(0, 0, 0, 0.2);
		background-color: rgb(178, 235, 255, 0.5);
	
	
/* 		padding-right: 7%;
		padding-left: 2%;
	padding-top: 2%;
	
		padding-bottom: 10%; */
		
		padding: 5%;
padding-left: 2%;


		margin-bottom: 20%;
	
	}
	
	
	.detail-white2{
/* 		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center; */
		
		background-color: #ffffff;
		height: 50%;
		width: 95%;
		border-radius: 20px;
		margin-left: 3%;
		padding-left: 3%;
		
	}
</style>