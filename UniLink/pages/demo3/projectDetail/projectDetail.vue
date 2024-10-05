<template>
	<view class="container-all">
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
				</view>
				<view class="task">
					<text class="content">任务列表</text>
					<br />
					<br />
					<checkbox-group class="uni-list" @change="checkboxChange" style="margin-left: 5%;">
						<label class="uni-list-cell uni-list-cell-pd" v-for="item in checkboxItems" :key="item.name">
							<view style="margin-top: 5%;">

								<checkbox :value="item.value" :checked="item.checked">{{ item.value }}</checkbox>
							</view>
							<!-- <view>{{ item.value }}</view> -->
						</label>
					</checkbox-group>
				</view>
				<button class="apply-buttom" @click="handleClick">申请加入项目</button>

			</view>




		</view>
		<view v-else>
			<text>未找到项目详情，请检查项目 ID 是否正确。</text>
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
		setup() {
			// 响应式数据，用于存储项目详细信息
			const project = ref(null);

			// 模拟的项目数据（可以从接口或数据库中获取）
			const projects = [{
					id: 1,
					name: '垃圾邮件过滤',
					domain: '机器学习',
					requiredTalent: '数学高手',
					mentor: '王石平',
					vacancies: 2,



					inroduction: "过滤垃圾邮件",
					target: "训练出最好的垃圾邮件过滤模型",
					member: ["宋浩", "李永乐"],
					taskList: ["模型选择", "模型部署", "训练", "测试"],




				},
				{
					id: 2,
					name: '自然语言至图查询语言翻译',
					domain: '人工智能',
					requiredTalent: '大模型微调',
					mentor: '郭坤',
					vacancies: 1,


					inroduction: "小样本条件下的自然与原至图查询语言翻译大模型微调",
					target: "参赛者需要使用提供的在TuGraph-DB上可执行的Cypher语料，对一个指定的本地模型进行微调。",
					member: ["朝兴财","章师月"],
					taskList: ["模型选择","微调"],
				},
				{
					id: 3,
					name: '跨专业项目合作app',
					domain: '软件工程',
					requiredTalent: '软件工程师，前后端设计',
					mentor: '张栋',
					vacancies: 3,

					inroduction: "UniLink 表达了连接不同学科、不同背景的大学生，建立跨学科交流的桥梁。",
					target: "根据用户需求，设计一个app",
					member: ["晨纸红"],
					taskList: ["原型设计","编程实现"],
				},
				{
					id: 4,
					name: '淘宝数据爬取',
					domain: '爬虫',
					requiredTalent: 'js逆向',
					mentor: '吴伶',
					vacancies: 4,

					inroduction: "该项目需要攻克淘宝的反爬机制，使我们能正常获取淘宝的数据",
					target: "爬取淘宝的商品数据，用作数据分析",
					member: ["无"],
					taskList: ["了解淘宝的反爬机制","逆向获取数据"],
				},
				{
					id: 5,
					name: '淘宝数据分析',
					domain: '大数据',
					requiredTalent: '数据分析师',
					mentor: '陈羽中',
					vacancies: 4,

					inroduction: "面对海量数据，需要对数据进行处理，然后对数据进行分析",
					target: "挖掘出数据的价值",
					member: ["朝星彩","账室阅"],
					taskList: ["部署大数据分析工具","对数据进行处理","提取数据价值"],
				},
			];

			onMounted(() => {
				// 使用 getCurrentPages 获取当前页面栈
				const pages = getCurrentPages();
				console.log(pages);
				const currentPage = pages[pages.length - 1]; // 获取当前页面实例
				console.log(currentPage);
				// 从当前页面实例中获取路由参数 options
				const options = currentPage.options;
				console.log(options);
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
					title: '申请已提交', // 提示内容
					icon: 'success', // 图标类型
					duration: 2000 // 持续时间，单位为毫秒
				});
			};
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
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);

		justify-content: center;
		align-items: center;
		/* 垂直居中 */
		height: 50vh;


	}



	.thick {
		font-weight: bold;
		font-size: 15px;

	}

	.container-all {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		/* 设置容器高度为整个屏幕高度*/
		width: 100vw;


	}



	.detail-white {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap:30px;
		background-color: #ffffff;
		height: 90%;
		/* 设置容器高度为整个屏幕高度*/
		width: 90%;
		border-radius: 20px;
		/* padding-left: 10px; */

	}

	.content {
		margin-top: 0px;
		padding: 0px 50px;
		font-size: 15px;
		color: #888888;
		letter-spacing: 0.6px;
		line-height: 1.6
	}

	.task {
		margin: 20px;
		padding-top: 10px;
		border-radius: 20px;
		border: 2px solid rgba(0, 0, 0, 0.3);
		height: 220px;
		/* 设置容器高度为整个屏幕高度*/
		width: 93%;
		margin-bottom: 10px;


	}

	.center-container {

		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		position: absolute;
		margin-bottom: 30px;
		height: 95%;
		/* 设置容器高度为整个屏幕高度*/
		width: 90%;
		overflow-y:auto;
		border-radius: 20px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
		background-color: rgb(178, 235, 255, 0.5);

	}

	.apply-buttom {

		font-size: 12px;

		width: 150px;
		/* 设置按钮的宽度 */
		height: 40px;

		/* position: absolute; */
		/* 设置按钮为绝对定位 */
		/* bottom: 35px; */
		/* 距离底部20px */
		/* right: 118px; */
		/* 距离右边20px */
		color: black;
		/* 按钮字体颜色 */
		/*border: none;  去除边框 */
		border-radius: 10px;
		/*圆角 */
		padding: 5px;
		/* 按钮内边距 */
		cursor: pointer;
		/* 鼠标悬停效果 */
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);

	}
</style>