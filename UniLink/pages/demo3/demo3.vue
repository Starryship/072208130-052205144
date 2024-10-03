<template>
	<view class="container">
		<!-- 搜索框 -->
		<view class="search-bar">
			<!-- 输入框 -->
			<input type="text" v-model="searchQuery" placeholder="请输入搜索内容" class="search-input" />
			<!-- 搜索图标 -->
			<image src="/static/pic/Leading icon.png" class="search-icon" @click="onSearch" mode="aspectFit"
				style="width: 40px; height: 40px;" />
		</view>


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
								<!-- 								<text>需求人才类型: {{ project.requiredTalent }}</text>
								<br>
								<view class="know-more-flex">
								<text>剩余空位: {{ project.vacancies }}</text>
								<text class="know-more-blue">了解详细</text>
								</view> -->
								<text>需求人才类型：</text><text style="text-decoration: underline;">{{ project.requiredTalent }}</text>

								<br>
								<text>导师：</text><text style="text-decoration: underline;">{{project.mentor}}</text>
								<br>
								<text>剩余空位：</text><text style="text-decoration: underline;">{{ project.vacancies }}</text>
								<text class="know-more-blue">了解详细</text>
							</view>
						</view>

					</scroll-view>
				</view>
				<!-- 				<view @tap="goTop" class="uni-link uni-center uni-common-mt">
					点击这里返回顶部
				</view> -->

			</view>
		</view>




	</view>
</template>

<script>
	import {
		ref,
		computed
	} from 'vue'; // 引入组合式 API 的 ref 和 computed 函数

	export default {
		setup() {
			// 定义搜索框内容的响应式数据
			const searchQuery = ref('');

			// 定义项目列表数据
			const projectList = ref([{
					name: '垃圾邮件过滤',
					domain: '机器学习',
					requiredTalent: '数学高手',
					mentor: '王石平',
					vacancies: 2
				},
				{
					name: '自然语言至图查询语言翻译',
					domain: '人工智能',
					requiredTalent: '大模型微调',
					mentor: '郭坤',
					vacancies: 1
				},
				{
					name: '跨专业项目合作app',
					domain: '软件工程',
					requiredTalent: '软件工程师，前后端设计',
					mentor: '张栋',
					vacancies: 3
				},
				{
					name: '淘宝数据爬取',
					domain: '爬虫',
					requiredTalent: 'js逆向',
					mentor: '吴伶',
					vacancies: 4
				},
				{
					name: '淘宝数据分析',
					domain: '大数据',
					requiredTalent: '数据分析师',
					mentor: '陈羽中',
					vacancies: 4
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

			return {
				searchQuery,
				onSearch,
				projectList,
				filteredProjects,
			};
		}
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
		background-color: #ffffff;
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
		font-size: 10px;
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
	
</style>