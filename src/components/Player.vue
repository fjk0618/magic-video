<template>
	<div class="player-box">
		<!-- 提示信息 -->
		<a-alert center style="margin-bottom: 12px;border-radius: var(--fillet);">若播放异常，请更换接口后重试</a-alert>

		<!-- 播放器 -->
		<div class="player">
			<transition name="fade">
				<div class="loader" v-if="load">
					<loader />
				</div>
			</transition>
			<iframe ref="videoIframe" width="100%" height="100%" allowtransparency="true" frameborder="0"
				allowfullscreen></iframe>
		</div>


		<!-- 解析组件 -->
		<div class="parse">
			<!-- 选择框 -->
      <div class="select-api">
        <a-select v-model="selectApi" size="large" placeholder="选择接口"
                  style="width: 100%; border-radius: var(--fillet);"
                  :trigger-props="{ autoFitPopupMinWidth: true }">
          <a-option v-for="item in options" :key="item.value" :value="item.value">
            {{ item.label }}
          </a-option>
        </a-select>
      </div>
			<!-- 输入框 -->
			<div class="input-url">
				<a-input size="large" v-model="inputUrl" placeholder="输入视频链接,点击解析播放" allow-clear
					style="border-radius: var(--fillet);">
					<template #prefix>
						<img src="/public/images/lj.svg" width="20px">
					</template>
				</a-input>
			</div>
			<!-- 按钮 -->
			<div v-if="playButton" class="button-play">
				<a-button @click="play" size="large" type="primary" long
					style="border-radius: var(--fillet);">解析播放</a-button>
			</div>
			<div v-if="stopButton" class="button-play">
				<a-button @click="stop" type="primary" size="large" status="warning" long
					style="border-radius: var(--fillet);">停止播放</a-button>
			</div>
		</div>

    <!-- 解析链接教程 -->
    <div>
      <div style="text-align: center; margin-top: 8px;">
      <span @click="showModal = true">
        <svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" stroke="currentColor" class="arco-icon arco-icon-question-circle" stroke-width="4" stroke-linecap="butt" stroke-linejoin="miter" style="color: rgb(22, 93, 255); margin-right: 4px;">
          <path d="M42 24c0 9.941-8.059 18-18 18S6 33.941 6 24 14.059 6 24 6s18 8.059 18 18Z"></path>
          <path d="M24.006 31v4.008m0-6.008L24 28c0-3 3-4 4.78-6.402C30.558 19.195 28.288 15 23.987 15c-4.014 0-5.382 2.548-5.388 4.514v.465"></path>
        </svg>什么是解析链接？查看教程</span>
      </div>

      <a-modal v-model:visible="showModal" title="教程" width="350px">
        <div class="jctk" style="text-align: center;">
          <span>教程</span>
          <a-collapse>
            <a-collapse-item header="什么是解析链接？如何免费观看？">
              <p>解析链接就是你去官方网站（例如爱奇艺、腾讯视频等）中复制你想要看的视频链接（不需要会员、不需要登录），然后将其粘贴到本站解析，然后就可以 <span class="highlight">免费观看视频</span>！</p>
            </a-collapse-item>
            <a-collapse-item header="步骤演示">
              <p>① 前往您想看的视频所在的平台的官方网站，如：<a target="_blank" href="https://www.iqiyi.com/">爱奇艺</a>、 <a target="_blank" href="https://v.qq.com/">腾讯视频</a>、<a target="_blank" href="https://youku.com/">优酷视频</a>、<a target="_blank" href="https://www.mgtv.com/">芒果TV</a>、 <a target="_blank" href="https://www.bilibili.com/">哔哩哔哩</a>等等 ...</p>
              <p>② 在官网中搜索您想看的视频（不需要登录），如：奔跑吧，进入其播放页面（<span class="highlight">重要！不是搜索完就结束，而是要在视频播放的页面复制链接！</span>）</p>
              <img src="/images/jxjc.png" alt="" style="width: 100%; max-width: 450px;">
              <p>③ 粘贴至本站，点击解析播放即可！(<span class="highlight">如无法观看，请在左侧更换接口尝试！</span>)</p>
            </a-collapse-item>
            <a-collapse-item header="通过名字搜索">
              <p>您亦可在下方输入影片名字来进行观影！</p>
            </a-collapse-item>
          </a-collapse>
        </div>
        <template #footer>
          <a-button @click="showModal = false">取消</a-button>
          <a-button type="primary" @click="showModal = false">确认</a-button>
        </template>
      </a-modal>
    </div>
    <!-- 搜索解析链接教程 -->
    <div class="card-box">
      <div style="text-align: center; margin: 0px auto 9px auto">
        <span style="color: blue; font-size: 17px">懒得找链接？搜索一下,即刻观看！</span>
      </div>
      
      
      <div class="search-bar">
        <a-input placeholder="输入影片关键词搜索..." style="margin-right: 8px; border-radius: var(--fillet); flex-grow: 1;" v-model="searchQuery" >
          <template #prefix>
            <icon-search style="color: rgb(var(--primary-6));" />
          </template>
        </a-input>
        <a-button type="primary" style="border-radius: var(--fillet);" @click="search(1)">搜索</a-button>
      </div>
      <a-tabs :active-key ="activeKey" @change="onTabChange" style="width: 100%;">
        <a-tab-pane key="1" title="搜索结果">
          <a-spin :loading="loading" style="width: 100%;" tip="努力搜索中">
            <a-table :columns="columns" :data="searchPageData" :pagination="false" class="search-results-table"  @row-click="searchDetail" />
            <Pagination v-if="searchPageData.length!=0"
                :current="searchCurrentPage"
                :total=searchPageTotal
                :size=searchPageSize
                showTotal
                        :onChange="search"
            />
          </a-spin>
        </a-tab-pane>
        <a-tab-pane key="2" title="影片详情">
          <a-spin :loading="loading" style="width: 100%;" tip="努力加载中">
            <a-result
                v-if="!selectedMovie"
                icon="info"
            >
              <template #title>
                影片详情为空<br />请前往搜索影片后选择对应的影片！
              </template>
            </a-result>
            <div v-else>
              <!-- 这里可以放置影片详情的展示内容 -->
              <div class="movie-detail">
                <div class="info_box">
                  <div class="img_box">
                    <img :src="`https://proxy.iprouter.top/${selectedMovie.image}`" alt="影片封面" class="movie-image" />
                  </div>
                  <div class="infoText">
                    <h2>{{ selectedMovie.title }}</h2>
                    <p><strong>介绍：</strong>{{ selectedMovie.description }}</p>
                  </div>
                </div>
                <div class="detail_box">
                  <div class="movie-info">
                    <div class="info-row">
                      <div class="info-label">上映时间</div>
                      <div class="info-value">{{ selectedMovie.releaseDate }}</div>
                      <div class="info-label">年份</div>
                      <div class="info-value">{{ selectedMovie.year }}</div>
                    </div>
                    <div class="info-row">
                      <!--                    <div class="info-label">版本</div>-->
                      <!--                    <div class="info-value">{{ selectedMovie.version }}</div>-->
                      <div class="info-label">更新至</div>
                      <div class="info-value">{{ selectedMovie.updatedTo }}</div>
                    </div>
                    <!--                  <div class="info-row">-->
                    <!--                    <div class="info-label">更新周期</div>-->
                    <!--                    <div class="info-value">{{ selectedMovie.updateCycle }}</div>-->
                    <!--                    <div class="info-label">状态</div>-->
                    <!--                    <div class="info-value">{{ selectedMovie.status }}</div>-->
                    <!--                  </div>-->
                    <div class="info-row">
                      <div class="info-label">类型</div>
                      <div class="info-value" colspan="3">{{ selectedMovie.type }}</div>
                    </div>
                  </div>
                </div>
                <div class="juji_box">
                  <h3>剧集列表（点击播放）：</h3>
                  <div class="episode-grid" style="margin-top: 20px">
                    <Button
                        v-for="episode in selectedMovie.episodes"
                        :key="episode.id"
                        :type="isSelected(episode) ? 'primary' : 'secondary'"
                        @click="playEpisode(episode)"
                    >
                      {{ episode.title }}
                    </Button>
                  </div>
                </div>
              </div>
            </div>
          </a-spin>
        </a-tab-pane>
        <a-tab-pane key="3" title="观影记录">
          <a-spin :loading="loading" style="width: 100%;" tip="努力加载中">
          <!-- 当没有观影记录时，显示提示 -->
          <a-result v-if="watchRecords.length === 0" icon="info-circle">
            <template #title>
              暂无观影记录
            </template>
          </a-result>

          <!-- 有观影记录时，显示列表 -->
          <a-list
              v-else
          >
            <template #footer>
              <div style="display: flex; justify-content: space-between; align-items: center;">
                <div style="flex: 1; text-align: center;">
                  仅保持最近的10条记录
                </div>
                <a-link status="danger" @click="clearWatchRecords">清空观影记录</a-link>
              </div>
            </template>
            <a-list-item v-for="watchRecord in watchRecords" :key="watchRecord">
              <a-list-item-meta
                  :title="watchRecord.title"
                  :description="`上次观看到：${watchRecord.episode.title}，时间：${watchRecord.timestamp}`"
              >
                <template #avatar>
                  <a-image
                      width="45px"
                      :alt="watchRecord.title"
                      :src="`https://proxy.iprouter.top/${watchRecord.image}`"
                  />
                </template>
              </a-list-item-meta>
              <template #actions>
                <!-- 按钮点击继续观看 -->
                <a-button type="primary" @click="continueWatching(watchRecord)">
                  继续观看
                </a-button>
              </template>
            </a-list-item>
          </a-list>
          </a-spin>
        </a-tab-pane>
      </a-tabs>
    </div>

	</div>
</template>

<script setup>
import {
	ref,onMounted
} from 'vue'
import axios from 'axios';
import { IconSearch } from '@arco-design/web-vue/es/icon';

import { Message,Pagination,Button } from '@arco-design/web-vue';
import loader from './Loader.vue'
const watchRecords = ref([]);
const activeKey = ref('1');
const loading = ref(false);
const selectedEpisode = ref(null);
const searchCurrentPage = ref(1);
const searchPageSize = ref(0);
const searchPageTotal = ref(0);
const searchPageData = ref([]);
const selectedMovie = ref(null);
const hitokoto = ref(null);
const searchQuery = ref('');
const playButton = ref(true)
const showModal = ref(false)
const stopButton = ref(false)
const inputUrl = ref('')
const load = ref(true)
const selectApi = ref('https://player.q168.vip/?url='); // 默认选择推荐接口
const options = [
  {
    value:
        'https://player.q168.vip/?url=',
    label: '默认高清',
  },
  {
    value:
        'https://jx.xmflv.com/?url=',
    label: '备用高清',
  },


]

onMounted(() => {
  loadWatchRecords();
});

const loadWatchRecords = () => {
  const records = JSON.parse(localStorage.getItem('watchRecords')) || [];
  watchRecords.value = records;
};

// 清空观影记录
const clearWatchRecords = () => {
  localStorage.removeItem('watchRecords');
  watchRecords.value = [];
};

// 继续观看
const continueWatching = async (record) => {
  try {
    loading.value = true;

    // 搜索详情
    await searchDetail({ vod_id: record.id });

    // 播放视频
    await playEpisode(record.episode);

    // 切换到播放器标签页
    activeKey.value = '2';
  } catch (error) {
    console.error('An error occurred:', error);
  } finally {
    loading.value = false;
  }
};


const columns = [
  { title: '名字', dataIndex: 'vod_name' },
  { title: '更新日期', dataIndex: 'vod_time' },
  { title: '最新期数', dataIndex: 'vod_remarks' },
  { title: '类型', dataIndex: 'type_name' },
];

const isSelected = (episode) => {
  return selectedEpisode.value && selectedEpisode.value.url === episode.url && selectedEpisode.value.title === episode.title;
}

const onTabChange = (key) => {
  activeKey.value=key;
}




const  searchDetail = async (data) => {
  loading.value = true;

  await axios({
    url: 'https://proxy.iprouter.top/http://zy.xiaomaomi.cc/api.php/provide/vod/?ac=detail&ids='+data.vod_id,
    method: 'GET',
  }).then(function (response) {
    var tem=response.data.list[0]
    var playUrls = tem.vod_play_url.split('#');
    var  episodes=[] ;
    playUrls.forEach((item, index) => {
      var parts = item.split('$');
      var title = parts[0];
      // if (!title.includes('第')) {
      //   title = '第' + title + '集';
      // }
      episodes.push({ id: index, title: title, url: parts[1] });
    });
    episodes.reverse();

    selectedMovie.value = {
      id: tem.vod_id,
      title: tem.vod_name,
      description: tem.vod_content,
      image: tem.vod_pic,
      releaseDate: tem.vod_time,
      year: tem.vod_year,
      version: tem.vod_version,
      updatedTo: tem.vod_remarks,
      status: tem.vod_state,
      type: tem.type_name,
      episodes: episodes,
    };
  }).catch(function () {
    console.log('请求失败');
  });

  activeKey.value='2';
  loading.value = false;
};


const playEpisode = (data) => {
  const selectApiValue = selectApi.value; // 获取选择框的值
  selectedEpisode.value=data;
  const finalUrl = selectApiValue + encodeURIComponent(data.url); // 拼接接口链接和输入框的值
  const iframe = document.querySelector('.player iframe');
  iframe.src = finalUrl;

  inputUrl.value=data.url;

  // 在播放视频时隐藏动画元素
  load.value = false;
  // 在播放视频时隐藏播放按钮
  playButton.value = false;
  // 在播放视频时显示停止按钮
  stopButton.value = true;

  Message.success('连接成功，请等待视频加载完成');

  // 获取当前日期和时间
  const now = new Date();

// 格式化日期和时间为 "YYYY-MM-DD HH:mm:ss"
  const timestamp = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')} ${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}:${String(now.getSeconds()).padStart(2, '0')}`;
  // 保存观影记录
  saveWatchRecord({
    id: selectedMovie.value.id,
    title: selectedMovie.value.title,
    image: selectedMovie.value.image,
    episode: data,
    timestamp: timestamp,
  });
};

// 保存观看记录
const saveWatchRecord = (record) => {
  let records = JSON.parse(localStorage.getItem('watchRecords')) || [];

  // 检查是否已经有相同记录，避免重复
  const existingRecordIndex = records.findIndex(r => r.episode.url === record.episode.url && r.episode.title === record.episode.title);
  if (existingRecordIndex !== -1) {
    records.splice(existingRecordIndex, 1); // 移除旧记录
  }

  records.unshift(record); // 添加新记录

  // 保持最近的10条记录
  if (records.length > 10) {
    records.pop();
  }

  localStorage.setItem('watchRecords', JSON.stringify(records));

  // 更新观影记录
  loadWatchRecords();
};



const search = async (page) => {

  if (searchQuery.value==''){
    Message.warning('请输入搜索内容');
    loading.value = false;
    return;
  }

  loading.value = true;
  activeKey.value='1';
  await axios({
    url: 'https://proxy.iprouter.top/http://zy.xiaomaomi.cc/api.php/provide/vod/?ac=list&wd=' + searchQuery.value + '&pg='+page,
    method: 'GET',
  }).then(function (response) {
    searchPageData.value = response.data.list;
    searchPageTotal.value = parseInt(response.data.total) ;
    searchPageSize.value =parseInt(response.data.limit) ;
    searchCurrentPage.value =parseInt(response.data.page) ;
  }).catch(function () {
    console.log('请求失败');
  });
  loading.value = false;
};




const play = () => {
	const selectApiValue = selectApi.value; // 获取选择框的值
	const videoUrl = inputUrl.value; // 获取输入框的值

	if (videoUrl) {
		const finalUrl = selectApiValue + encodeURIComponent(videoUrl); // 拼接接口链接和输入框的值

		const iframe = document.querySelector('.player iframe');
		iframe.src = finalUrl;

		Message.success('连接成功，请等待视频加载完成');
		// 在播放视频时隐藏动画元素
		load.value = false;
		// 在播放视频时隐藏播放按钮
		playButton.value = false;
		// 在播放视频时显示停止按钮
		stopButton.value = true;
	} else {
		Message.warning('请输入视频链接');
	}
}
const stop = () => {
	const iframe = document.querySelector('.player iframe');
	iframe.src = '';
	load.value = true; // 在停止播放时显示动画元素
	playButton.value = true; // 在停止播放时显示播放按钮
	stopButton.value = false; // 在停止播放时隐藏停止按钮
	Message.info('已停止播放，继续看点啥？');
}
</script>


<style scoped>

.card-box {
  margin-top:20px ;
  width: 100%;
}

.search-bar {
  display: flex;
  align-items: center;
}

.search-results-table {
  width: 100%;
}

.player-box {
	transition: all 0.2s ease-in-out;
	background-color: var(--color-bg-2);
	padding: 12px;
	border-radius: var(--fillet);
	box-shadow: var(--shadow);

	@media only screen and (max-width:768px) {
		margin-top: 0px;
	}
}

.player {
	width: 100%;
	height: 350px;
	border-radius: var(--fillet);
	background: #000;
	position: relative;

	@media only screen and (max-width:768px) {
		height: 250px;

	}

}

.player iframe {
	border-radius: var(--fillet);
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
}

.loader {
	width: 100%;
	height: 100%;
	border-radius: var(--fillet);
	background-image: url('/images/zmbg.webp');
	background-repeat: no-repeat;
	background-size: 100% 100%;

}

.parse {
	display: flex;
	flex-direction: row;
	width: 100%;
	margin-top: 10px;

	@media only screen and (max-width:768px) {
		flex-direction: column;
	}
}

/* 选择框 */
.select-api {
	width: 20%;

	@media only screen and (max-width:768px) {
		width: 100%;
	}
}

/* 输入框 */
.input-url {
	width: 60%;
	margin: 0 6px;

	@media only screen and (max-width:768px) {
		width: 100%;
		margin: 12px 0;
	}
}

/* 按钮 */
.button-play {
	width: 20%;

	@media only screen and (max-width:768px) {
		width: 100%;
	}
}

.option-text {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.fade-enter,
.fade-leave-to

/* .fade-leave-active 在Vue 2.1.8+中 */
	{
	opacity: 0;
}

.movie-detail {
  padding: 16px;
}

.info_box {
  display: flex;
  margin-bottom: 16px;
}

.img_box {
  margin-right: 16px;
}

.movie-image {
  border-radius: var(--fillet);
  width: 200px;
  height: auto;
}

.infoText {
  flex-grow: 1;
}

.detail_box {
  margin-bottom: 16px;
}

.movie-info {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 8px;
}

.info-row {
  display: contents;
}

.info-label {
  font-weight: bold;
  text-align: left;
}

.info-value {
  text-align: left;
}

.juji_box {
  margin-top: 16px;
}

.episode-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.hito {
  font-size: 15px;
  display: flex;
  align-items: center;
  margin-bottom: 12px;
  white-space: nowrap;
}

.hito-title {
  background-color: var(--color-fill-2);
  color: var(--color-text-2);
  padding: 4px 8px;
  margin-right: 10px;
  border-radius: 6px;
  font-size: 12px;
  display: flex;
  align-items: center;
}

.hito-text {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

}

.hito-from {
  display: block;
  unicode-bidi: isolate;
}
</style>
