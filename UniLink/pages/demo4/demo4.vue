<template>
	<view class="container">


		<view>
			<view class="uni-padding-wrap uni-common-mt">
				<view>
					<scroll-view :scroll-top="scrollTop" scroll-y="true" class="scroll-Y" @scrolltoupper="upper"
						@scrolltolower="lower" @scroll="scroll">
						<!-- 卡片式项目展示 -->
						<view class="project-card" v-for="(project, index) in filteredProjects" :key="index">
							<view class="project-header">
								<text class="project-name">{{ project.name }}</text>
								<text class="project-domain">{{ project.domain }}</text>
							</view>
							<view class="project-info">

<!-- 								<text>需求人才类型：</text><text style="text-decoration: underline;">{{ project.requiredTalent }}</text>

								<br> -->
								<text>导师：</text><text style="text-decoration: underline;">{{project.mentor}}</text>
								<br>
								<span class="thick">已加入队员：</span><text v-for="(member, index) in project.member"
									:key="index">{{ member }}<span v-if="index < project.member.length - 1">, </span></text>
		
								<br>
								<text>时间规划：</text><text style="text-decoration: underline;">{{ project. time}}</text>
								
								
								<!-- <text class="know-more-blue"><span @tap="pr_know_more()">了解详细>></span></text> -->
								
								<text class="know-more-blue" @tap="pr_know_more(project.id)">管理项目>></text>
								
							</view>
						</view>

					</scroll-view>
				</view>

				

			</view>
		</view>
		
		
		<button class="floating-button" @click="handleClick2">+</button>




	</view>
</template>

<script>
	import {
		ref,
		computed,
		onMounted,
	} from 'vue'; // 引入组合式 API 的 ref 和 computed 函数
// import { useRoute } from 'vue-router';
import { getCurrentInstance } from 'vue';

import { onLoad } from '@dcloudio/uni-app'

	export default {
		setup() {
			// 定义搜索框内容的响应式数据
			const searchQuery = ref('');
// const route = useRoute();



			// 定义项目列表数据
			const projectList = ref([


				{
					id: 1,
					name: '福uu',
					domain: '软件工程',
					requiredTalent: '软件工程师',
					mentor: '翁谦',
					time:"2023.10.12-2025.10.12",

					inroduction: "福uu是FZU校内app，用于提供课程信息等",
					target: "分析需求，设计app",
					member: ["Bob","晨纸红","朝兴财","章师月"],
					taskList: ["原型设计","编程实现"],
				},
				
				{
					id: 2,
					name: '联邦学习',
					domain: '分布式',
					requiredTalent: '大数据，机器学习',
					mentor: '郭坤',
					time:"2024.10.22-2025.05.12",
				
					inroduction: "一个机器学习框架，能有效帮助多个机构在满足用户隐私保护、数据安全和政府法规的要求下，进行数据使用和机器学习建模。",
					target: "优化算法",
					member: ["Bob"],
					taskList: ["横向联邦学习","纵向联邦学习","联邦迁移学习"],
				},



			]);

			// 计算属性：根据搜索框内容动态过滤项目列表
			const filteredProjects = computed(() => {
				if (searchQuery.value.trim() === '') {
					return projectList.value; // 如果搜索框为空，显示全部项目
				}
				// 过滤逻辑：根据项目名称或领域匹配搜索关键词
				return projectList.value.filter(project =>
					project.name.includes(searchQuery.value) ||
					project.domain.includes(searchQuery.value)
				);
			});

			// 定义搜索逻辑
			const onSearch = () => {
				console.log('搜索内容：', searchQuery.value); // 控制台输出当前搜索内容
				uni.showToast({
					title: `正在搜索：${searchQuery.value}`,
					icon: 'none'
				});
			};
			

			
		    const pr_know_more = (id) => {
		      // 跳转到指定的项目详情页面，并传递项目 ID 作为查询参数
		      uni.navigateTo({
		        url: `/pages/demo4/demoManage/demoManage?id=${id}`,
		        success: () => {
		          console.log(`成功跳转到项目 ID 为 ${id} 的详情页面`);
		        },
		        fail: (err) => {
		          console.error('跳转失败：', err);
		        }
		      });
		    };
			
			

			

			const handleClick2 = () => {
				//    alert('按钮被点击了');

				// 显示提示框
				// uni.showToast({
				// 	title: '申请已提交', // 提示内容
				// 	icon: 'success', // 图标类型
				// 	duration: 2000 // 持续时间，单位为毫秒
				// });
				
				
				uni.navigateTo({
				  url: `/pages/demo5/demo5`,
				  success: () => {
				    console.log(`成功跳转到项目 ID 为 ${id} 的详情页面`);
				  },
				  fail: (err) => {
				    console.error('跳转失败：', err);
				  }
				});
};


	

	
	
	    const fetchData = () => {
	      uni.getStorage({
	        key: 'projectData',
	        success: function(res) {
	          if (res.data) {
	            projectList.value.push(JSON.parse(res.data)); // 将数据解析并赋值给 projectList
	          }
	        },
	        fail: function(err) {
	          console.error('获取数据失败:', err);
	        }
	      });
	    };
	
	    fetchData(); // 调用函数获取数据





			return {
				searchQuery,
				onSearch,
				projectList,
				filteredProjects,
				pr_know_more,
				handleClick2,

			};
		},
		

		
		
		
		
	};
</script>

<style scoped>
	.container {
		padding: 10px;
	}

	.search-bar {
		display: flex;
		align-items: center;
		margin: 20px 0;
		padding: 3px;
		border: 1px solid #ccc;
		border-radius: 5px;
		background-color: #fff;
	}

	.search-input {
		flex: 1;
		border: none;
		outline: none;
		padding: 5px;
	}

	.search-icon {
		width: 24px;
		height: 24px;
		margin-left: 10px;
	}

	.project-card {
		background-color: rgb(42,173,255,0.1);
		padding: 20px;
		margin-bottom: 15px;
		border: 1px solid #e0e0e0;
		border-radius: 10px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
	}

	.project-header {
		display: flex;
		justify-content: space-between;
		margin-bottom: 10px;

	}

	.know-more-flex {
		display: flex;
		justify-content: space-between;
		margin-bottom: 5px;
	}

	.text-underline {
		text-decoration: underline;
		/* 添加下划线 */
	}

	.know-more-blue {
		display: flex;
		justify-content: flex-end;
		color: #4493f8;
		font-size: 15px;
	}

	.project-name {
		font-size: 18px;
		font-weight: bold;
	}

	.project-domain {
		font-size: 14px;
		color: #888888;
	}

	.project-info {
		font-size: 16px;
		line-height: 2;
		color: #333333;
	}
	
	
	.floating-button {
	  position: fixed; /* 固定定位 */
	  bottom: 12%; /* 距离底部 20px */
	  right: 10%; /* 距离右侧 20px */
	  width: 60px; /* 按钮宽度 */
	  height:60px; /* 按钮高度 */
	  border-radius: 50%; /* 圆形按钮 */
	  background-color: #1e90ff; /* 按钮颜色 */
	  color: white;
	  border: none;
	  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2); /* 按钮阴影效果 */
	  font-size: 40px; /* 按钮内文字大小 */
	  cursor: pointer;
	  z-index: 999; /* 保证按钮处于最上层 */
	  
  text-align: center; /* 水平居中 */
  line-height: 60px; /* 文字高度等于按钮高度，实现垂直居中 */
	}
	
	
</style>