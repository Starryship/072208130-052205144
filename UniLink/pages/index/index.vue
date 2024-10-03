<template>
	<view class="content">
		<view class="text-area">欢迎使用UniLink</view>
		<image class="logo" src="/static/applethrow.png"></image>
		<br />
		<br />
		<view class="text-area">
			寻找志同道合的伙伴...
			
		</view>
		<br />
		<br />
		<view class="text-area">
			君有奇才我不贫。——(清)郑板桥
		</view>
		<br />
		<br />
		<br />
		<navigator url="/pages/demo1/demo1">
			<button class="btn">进入UniLink</button>
		</navigator>
	</view>
</template>

<script setup >

</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36px;
		color: #8f8f94;
	}
	.btn{
		width:500rpx;
		font-size: 28rpx;
		background: rgb(42,173,255);
		color: #fff;
		height: 90rpx;
		line-height: 90rpx;
		border-radius: 50rpx;
		box-shadow: 0 5px 7px 0 rgba(42,173,255,0.2);
	}
	.text{
		margin-bottom: 10px;
	}
</style>
