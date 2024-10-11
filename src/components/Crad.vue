<template>
	<div class="crad-box">
		<a-divider :size="2" orientation="center" type="dashed">
			<p>支持解析</p>
		</a-divider>
		<a v-for="(site, index) in crad" :key="index" :href="site.link" target="_blank" class="card">
			<div class="img">
				<img :src="site.logo" width="30px" />
			</div>
			<div class="textBox">
				<div class="textContent">
					<p class="h1">{{ site.name }}</p>
				</div>
			</div>
		</a>
    <div>
      <a-divider :size="2" orientation="center" type="dashed">
        免责声明
      </a-divider>
      <div class="container">
        <p style="font-size: 12px">本站只提供视频播放服务，仅供学习交流！一切资源来自于互联网，并不提供视频资源存储，也不参与录制和上传。</p>
        <p style="font-size: 12px">若本站的资源无意侵犯了您的版权，请联系我们，我们会及时回复和处理，谢谢！</p>
        <p style="font-size: 12px">管理员邮箱：<a href="mailto:1670046716@qq.com">1670046716@qq.com</a></p>
      </div>
    </div>
	</div>
</template>

<script setup>
import crad from '../assets/Crad.json';
</script>

<style scoped>


.container {
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
  text-align: center;
  font-size: 12px;
  color: #666;
  margin: 0;
}
.container p {
  font-size: 16px;
  line-height: 1.6;
  color: #333;
  margin-bottom: 10px;
}
.container p:last-child {
  font-size: 14px;
  color: #666;
}
.container a {
  color: #007BFF;
  text-decoration: none;
}
.container a:hover {
  text-decoration: underline;
}
p {
	font-size: 15px;
}

.crad-box {
	transition: all 0.2s ease-in-out;
	display: flex;
	flex-direction: row;
	justify-content: space-around;
	flex-wrap: wrap;
	margin-top: 12px;
	background-color: var(--color-bg-2);
	padding: 12px;
	border-radius: var(--fillet);
	box-shadow: var(--shadow);

	@media only screen and (max-width:768px) {
		justify-content: flex-start;
	}
}

.card {
	width: 21%;
	height: 40px;
	border-radius: 6px;
	display: flex;
	align-items: center;
	justify-content: left;
	margin: 6px 0;


	@media only screen and (max-width:768px) {
		width: 33%;
	}

}

.img {
	width: 30px;
	height: 30px;
	margin-left: 10px;
}



.textBox {
	margin-left: 10px;
}

.textContent {
	display: flex;
	align-items: center;
	justify-content: space-between;
}

.h1 {
	font-size: 14px;
	font-weight: bold;
	color: var(--color-text-2);

	@media only screen and (max-width:768px) {
		font-weight: 100;
	}
}
</style>
