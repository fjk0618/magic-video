<template>
	<div class="card">

		<div class="info">
			<div>
				<p class="name">
					魔法解析
				</p>
				<p class="function">
          全网影视免费在线解析观看
				</p>
			</div>
			<div>
				<img src="/images/vite.svg" />
			</div>
		</div>

		<div class="butt">
			<button @click="Follow" class="request" type="button">
				<span>关注</span>
			</button>
			<button @click="Reward" class="request" type="button">
				<span>打赏</span>
			</button>
			<button @click="More" class="request2" type="button">
				<icon-star style="margin-right: 5px; font-size: 18px;" spin />
				<span>更多精彩</span>
			</button>
		</div>

		<!-- 关注弹出卡片 -->
		<a-modal width="auto" :simple="true" v-model:visible="gz" @ok="FollowOk" @cancel="FollowCancel">
			<template #title>
        <div class="arco-modal-body">
          <div  style="text-align: center;">
            <h3>牢记本站:{{currentUrl}}</h3>
          </div>
        </div>
			</template>
		</a-modal>
		<!-- 打赏弹出卡片 -->
		<a-modal width="240px" :simple="true" v-model:visible="ds" @ok="RewardOk" @cancel="RewardCancel">
			<template #title>
        <div  style="text-align: center;">
          让网站更好的运行下去<br>
          感谢您的支持
        </div>

			</template>
			<div class="ds">
				<a-tabs default-active-key="1" :animation="true">
					<a-tab-pane key="1">
						<template #title>
							<icon-wechatpay style="font-size: 16px;margin-right: 5px;" />
							<span>微信</span>
						</template>
						<img width="300" src="/images/wxskm.jpg" />
					</a-tab-pane>
					<a-tab-pane key="2">
						<template #title>
							<icon-alipay-circle style="font-size: 16px;margin-right: 5px" />
							<span>支付宝</span>
						</template>
						<img width="300" src="/images/zfbskm.jpg" />
					</a-tab-pane>
				</a-tabs>
			</div>
		</a-modal>
		<!-- 更多弹出卡片 -->
		<a-modal width="auto" :simple="true" v-model:visible="gd" @ok="MoreOk" @cancel="MoreCancel">
      <template #title>
        <div class="arco-modal-body">
          <div  style="text-align: center;">
            <h3>牢记本站:{{currentUrl}}</h3>
            <h3>后续更加精彩</h3>
          </div>
        </div>
      </template>
		</a-modal>

	</div>
</template>

<script setup>
import {
	ref
} from 'vue';
import { IconStar, IconWechatpay, IconAlipayCircle, IconScan } from '@arco-design/web-vue/es/icon';

const gz = ref(false);
const ds = ref(false);
const gd = ref(false);
const currentUrl = ref(window.location.href);
// 关注卡片配置
const Follow = () => {
	gz.value = true;
};
const FollowOk = () => {
	gz.value = false;
};
const FollowCancel = () => {
	gz.value = false;
};
// 打赏卡片配置
const Reward = () => {
	ds.value = true;
};
const RewardOk = () => {
	ds.value = false;
};
const RewardCancel = () => {
	ds.value = false;
};
// 更多卡片配置
const More = () => {
	gd.value = true;
};
const MoreOk = () => {
	gd.value = false;
};
const MoreCancel = () => {
	gd.value = false;
};
</script>

<style scoped>
.card {
	transition: all 0.2s ease-in-out;
	width: 48.5%;
	background-color: var(--color-bg-2);
	border-radius: var(--fillet);
	box-shadow: var(--shadow);
	padding: 15px;
}

.gz {
	text-align: center;

	img {
		width: 150px;
	}
}

.ds {
	text-align: center;

	img {
		width: 170px;
	}
}

.gd {
	text-align: center;

	img {
		width: 150px;
	}
}

.info {
	flex: 1;
	display: flex;
	justify-content: space-between;

	img {
		width: 40px;
		margin-right: 5px;
	}


	@media only screen and (max-width:768px) {

		img {
			width: 35px;
			margin-right: 0px;
		}
	}
}

.name {
	font-size: 21px;
	margin-bottom: 5px;
	font-weight: 500;
	color: var(--color-text-1);

	@media only screen and (max-width:768px) {
		font-size: 16px;
	}
}

.function {
	font-size: 13px;
	color: var(--color-text-2);

	@media only screen and (max-width:768px) {
		font-size: 12px;
	}
}

.butt {
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	flex-wrap: wrap;
}

.request {
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: 12px;
	width: 48%;
	border: 1px solid transparent;
	border-radius: var(--fillet);
	height: 32px;
	font-size: 14px;
	transition: all .3s ease;
	background-color: rgb(118, 36, 194);
	color: #fff;

}

.request2 {
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: 12px;
	width: 100%;
	border: 1px solid transparent;
	border-radius: var(--fillet);
	height: 32px;
	font-size: 14px;
	transition: all .3s ease;
	background-color: rgb(118, 36, 194);
	color: #fff;



	img {
		width: 20px;
		margin-right: 5px;
	}


}

.request:hover {
	background-color: rgb(133, 89, 194);
}

.request2:hover {
	background-color: rgb(133, 89, 194);
}
</style>
