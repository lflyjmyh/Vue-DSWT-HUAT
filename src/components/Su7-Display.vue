```vue
<template>
  <div>
    <nav class="top-nav">
      <div class="nav-logo-container">
        <div class="nav-logo" @click="navigateToMusic">
          <img src="/picture/XiaomiEV.jpg" alt="小米汽车LOGO">
        </div>
        <button v-if="activeSection === 'music'" class="back-button" @click="navigateToMain">返回</button>
      </div>
      <div class="nav-menu">
        <div class="search-container">
          <input
            type="text"
            class="search-input"
            placeholder="搜索SU7特性..."
            v-model="searchQuery"
            @keypress.enter="performSearch"
            @input="clearSearchResults"
          >
          <button class="search-button" @click="performSearch">搜索</button>
          <div class="search-results" :class="{ active: showSearchResults }">
            <p
              v-for="(result, index) in searchResults"
              :key="index"
              @click="navigateToSection(result.sectionId)"
            >
              <strong>{{ result.name }}</strong>: {{ result.description }}
            </p>
            <p v-if="searchError" class="search-error">{{ searchError }}</p>
          </div>
        </div>
      </div>
    </nav>

    <div class="content-container">
      <section id="su7" class="content-section" :class="{ active: activeSection === 'su7' }">
        <div class="content-block image-block">
          <img src="/picture/su7_1.jpg" alt="小米SU7外观展示">
        </div>
        <div class="content-block video-block">
          <video controls muted loop @error="handleVideoError" ref="video">
            <source src="/media/Xiaomi Su7.mp4" type="video/mp4">
            <p>您的浏览器不支持视频播放。</p>
          </video>
          <div class="video-error" v-show="videoError">视频加载失败，请检查文件路径。</div>
        </div>
        <div class="content-block image-block" id="car-color">
          <div class="carousel-container">
            <div class="carousel-track" :style="{ transform: `translateX(-${carColorIndex * 100}%)` }">
              <div
                v-for="(color, index) in carColors"
                :key="index"
                class="carousel-item"
                :data-color="color.name"
              >
                <img :src="color.src" :alt="color.name + '配色'">
              </div>
            </div>
            <div class="color-name" :class="{ active: showCarColorName }">{{ carColors[carColorIndex]?.name }}</div>
            <div class="carousel-indicators">
              <span
                v-for="(color, index) in carColors"
                :key="index"
                :class="{ active: index === carColorIndex }"
                @click="goToCarColor(index)"
              ></span>
            </div>
            <button class="carousel-prev" @click="prevCarColor">‹</button>
            <button class="carousel-next" @click="nextCarColor">›</button>
          </div>
        </div>
        <div class="content-block text-block" id="introduction">
          <p>
            小米SU7是小米集团推出的首款高端纯电动轿跑车型，定位中高端新能源汽车市场。<br>
            该车动力性能表现突出：双电机四驱版本最大功率达<strong>495kW</strong>，峰值扭矩<strong>838N·m</strong>，百公里加速仅需<strong>2.78秒</strong>（Max性能版）。<br>
            智能驾驶系统采用Xiaomi Pilot方案，配备1颗激光雷达、11颗高清摄像头、3颗毫米波雷达及12颗超声波雷达，硬件算力达508TOPS。<br>
            车机系统搭载小米澎湃OS2，支持与手机、智能家居设备的深度互联，中控16.1英寸3K分辨率屏幕可拓展智能香氛、对讲机等硬件配件。
          </p>
        </div>
        <div class="content-block image-block" id="outside-page">
          <div class="content-block text-block">
            <strong>优雅前脸</strong><br>
            <img src="/picture/su7_3_1.jpg" alt="优雅前脸">
            175°涟漪曲面打造出富有张力的肌肉感前轮包，配合1.36倍黄金宽高比，整车前脸动感优雅
          </div>
          <div class="content-block text-block">
            <strong>饱满尾部</strong><br>
            <img src="/picture/su7_3_2.jpg" alt="饱满尾部">
            圆润饱满的尾部线条，配合运动感十足的后扰流和辨识度极高的家族式光环尾灯，让速度与优雅并存
          </div>
          <div class="content-block text-block">
            <strong>流畅侧面</strong><br>
            <img src="/picture/su7_3_1.jpg" alt="流畅侧面">
            遵循 3 倍轮轴比和 2 倍轮高比的设计比例，舒展的流畅腰线与条背滑的设计，侧面观感流畅自然
          </div>
        </div>
        <div class="content-block image-block" id="inside-color">
          <div class="carousel-container">
            <div class="carousel-track" :style="{ transform: `translateX(-${insideColorIndex * 100}%)` }">
              <div
                v-for="(color, index) in insideColors"
                :key="index"
                class="carousel-item"
                :data-feature="color.name"
              >
                <img :src="color.src" :alt="color.name">
              </div>
            </div>
            <div class="feature-name" :class="{ active: showInsideColorName }">{{ insideColors[insideColorIndex]?.name }}</div>
            <div class="carousel-indicators">
              <span
                v-for="(color, index) in insideColors"
                :key="index"
                :class="{ active: index === insideColorIndex }"
                @click="goToInsideColor(index)"
              ></span>
            </div>
            <button class="carousel-prev" @click="prevInsideColor">‹</button>
            <button class="carousel-next" @click="nextInsideColor">›</button>
          </div>
        </div>
        <div class="content-block image-block" id="driving-space">
          <div style="text-align:center">
            <strong>C级驾驶空间</strong>
            <img src="/picture/su7_5_1.jpg">
            <em>3000mm 超长轴距 带来宽敞的车内乘坐空间充裕的头部和腿部空间 前后排都有自在的乘坐体验</em>
          </div>
        </div>
        <div class="content-block image-block" id="ergonomic-seats">
          <div style="text-align:center">
            <strong>人体工学座椅</strong>
            <img src="/picture/su7_5_2.jpg">
            <em>座椅采用多层三明治结构坐垫 针对性优化腰部和脊椎承托 Nappa 真皮包裹，触感柔软舒适</em>
          </div>
        </div>
        <div class="content-block image-block" id="insulated-glass">
          <div style="text-align:center">
            <strong>全车隔热玻璃</strong>
            <img src="/picture/su7_5_3.jpg">
            <em>双层镀银天幕、三层镀银前风挡玻璃，前后侧窗防晒隔热涂层。全车5.35m²整车玻璃面积，视野通透明亮且夏季不怕晒</em>
          </div>
        </div>
        <div class="content-block image-block" id="storage-space">
          <p style="text-align: center"><em>全车丰富储物空间 <br>处处都是收纳巧思</em></p>
          <img src="/picture/su7_6_1.jpg">
          <p>支持无线快充的手机放置位<br>前排门板储物盒2个雨伞收纳格<br><strong>超大</strong>手套箱</p>
          <div class="image-display-container">
            <div class="image-display-left">
              <img src="/picture/su7_6_2.jpg" alt="小米SU7展示1">
            </div>
            <div class="image-display-right">
              <img src="/picture/su7_6_3.jpg" alt="小米SU7展示2">
              <img src="/picture/su7_6_4.jpg" alt="小米SU7展示3">
            </div>
          </div>
          <br>
          <p>车载智能冰箱4.6L<br>后座雨伞储物槽<br>后排座椅双杯托</p>
          <br>
          <div class="image-display-container">
            <div class="image-display-left">
              <img src="/picture/su7_6_5.jpg" alt="小米SU7展示4">
            </div>
            <div class="image-display-right">
              <img src="/picture/su7_6_6.jpg" alt="小米SU7展示5">
              <img src="/picture/su7_6_7.jpg" alt="小米SU7展示6">
            </div>
          </div>
        </div>
        <div class="content-block image-block" id="trunk-space">
          <p><strong>超大前后备箱</strong><br>C级宽大车身和优秀的车身结构设计，带来傲视同级的 105L 超大前备箱和 517L 超大后备厢，空间规整布局合理，满足多样化的随车储物需求。</p>
          <div class="dual-image-display-container">
            <div class="dual-image-display-left">
              <img src="/picture/su7_7_1.jpg" alt="小米SU7展示左">
            </div>
            <div class="dual-image-display-right">
              <img src="/picture/su7_7_2.jpg" alt="小米SU7展示右">
            </div>
          </div>
        </div>
        <div class="content-block text-block" id="intelligent-driving">
          <p><strong>全系标配小米辅助驾驶</strong></p>
          <div class="content-block image-block">
            <img src="/picture/su7_10.jpg">
            <p><em>* 传感器感知范围仅为示意；小米辅助驾驶软硬件区分车型配置，场景功能以实际交付为准。 <br>* 辅助驾驶功能并不能代替驾驶员控制车辆，驾驶员始终承担着安全驾驶车辆与符合道路交通安全法律法规的全部责任。驾驶员在驾驶车辆过程中请务必全程保持注意力集中，时刻关注车辆周围路况，并做好随时干预车辆的准备。切勿依靠辅助驾驶功能来应对突发的紧急情况，否则可能导致严重伤害或死亡。请阅读用户手册中关于辅助驾驶功能的全部章节，了解辅助驾驶功能的限制情况。驾驶员在使用前应意识到这些限制情况，在交通状况复杂多变、冰雪雨路面湿滑天气、道路积水或烂泥路面、能见度较差、崎岖山路或高速路入口出口等情况下，请谨慎使用。</em></p>
          </div>
        </div>
        <div class="content-block text-block" id="accessories">
          <p><strong>丰富的车载生态配件 <br>功能扩展，体验进阶</strong></p>
          <div class="content-block image-block">
            <img src="/picture/su7_11.jpg">
          </div>
        </div>
        <div class="content-block text-block" id="motor-engine">
          <div class="content-block image-block">
            <p>小米超级电机<br>XiaomiHyperEngine</p><br>
            <img src="/picture/xsf_4_1.jpg">
            <em>电机，是新能源车的心脏<br>转速，决定了这颗心的性能<br>一骑绝尘的澎湃动力背后 <br>是三款高转速小米超级电机<br>由<strong>21000rpm</strong>开始<br>更有<strong>27200rpm</strong>挑战极限<br>你对速度不顾一切的信仰<br>我用实力不惜代价去守护</em>
          </div>
          <div class="content-block image-block">
            <p>小米超级电机V6系列<br>全球领先团队联合研发<br><strong>21000rpm</strong></p>
            <img src="/picture/xsf_4_2.jpg">
            <em>小米超级电机v6&小米超级电机v6s<br>小米首款超级电机 V6 系列，引入AI仿真技术，超 20 万次拓扑寻优， <br>转速达到惊人的 <strong>21000rpm</strong>。加速更快、极速更高、能耗更低，同级领先。</em>
          </div>
          <div class="content-block image-block">
            <h3>小米超级电机V8s</h3>
            <p>前所未有的性能革命</p>
            <img src="/picture/xsf_4_3.jpg">
            <em>一起见证，小米超级电机 V8s 挑战行业转速新巅峰。全新开发超高强度硅钢材料， <br>将转子结构、散热系统全部重新设计，带来史无前例的转速飞跃，功率高达 <strong>425kW</strong>。</em>
          </div>
        </div>
        <div class="content-block text-block" id="battery">
          <div class="content-block image-block">
            <h3>小米800V<br>碳化硅高压平台</h3>
            <img src="/picture/xsf_5_1.jpg">
            <em>CTB 一体化电池技术<br>77.8% 全球最高体积效率</em>
          </div>
        </div>
        <div class="content-block text-block" id="safety">
          <div class="content-block image-block">
            <h3>车云协同安全预警系统<br>实现 ASIL-D 功能安全等级</h3>
            <img src="/picture/xsf_5_2.jpg">
            <em>每秒检测超过 800 个信号，数据量是国标的 2 倍，实现 24小时 全天守候。<br>采用小米全栈自研的电池管理软件，紧急情况下 4ms 内切断电流，保证车内安全。</em>
          </div>
        </div>
        <div class="content-block text-block" id="versions">
          <div class="content-block image-block">
            <div class="three-image-display-container">
              <div class="three-image-item">
                <img src="/picture/su7_12_1.jpg" alt="小米SU7">
              </div>
              <div class="three-image-item">
                <img src="/picture/su7_12_2.jpg" alt="小米SU7pro">
              </div>
              <div class="three-image-item">
                <img src="/picture/su7_12_3.jpg" alt="小米SU7max">
              </div>
            </div>
          </div>
        </div>
        <div class="content-block image-block" id="pre-driving">
          <div class="action-image-container">
            <img src="/picture/su7_8.jpg" alt="小米SU7试驾与选配">
            <div class="button-container">
              <a href="https://www.xiaomiev.com/drive" class="reserve-button">预约试驾</a>
              <a href="https://www.xiaomiev.com/configurator/select-version?itemId=500001000" class="config-button">开始选配</a>
            </div>
          </div>
        </div>
        <div class="content-block image-block">
          <img src="/picture/Xiaomi HyperOS.jpg">
        </div>
        <div class="single-button-container" id="More">
          <a href="https://gamemcu.com/su7/" class="more-button">More</a>
        </div>
      </section>

      <section id="music" class="content-section" :class="{ active: activeSection === 'music' }">
        <div class="content-block music-content">
          <audio id="music-audio" loop ref="audio" controls>
            <source src="/media/Are You OK.mp3" type="audio/mp3">
            <p>您的浏览器不支持音频播放。</p>
          </audio>
          <div
            class="audio-play-pause"
            :class="{ play: !isPlaying, pause: isPlaying }"
            @click="toggleAudio"
          ></div>
          <h2>Powered by</h2>
          <img src="/picture/Main.png" alt="图片">
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// Search functionality
const searchQuery = ref('');
const showSearchResults = ref(false);
const searchResults = ref([]);
const searchError = ref('');

const su7Features = [
  { name: '简介', description: '双电机四驱，最大功率495kW，百公里加速2.78秒', sectionId: 'introduction' },
  { name: '智能驾驶', description: 'Xiaomi Pilot，配备激光雷达和508TOPS算力', sectionId: 'intelligent-driving' },
  { name: '车机系统', description: '小米澎湃OS2，16.1英寸3K屏幕', sectionId: 'power-performance' },
  { name: '储物空间', description: '105L前备箱，517L后备厢', sectionId: 'storage-space' },
  { name: '座椅设计', description: '人体工学座椅，Nappa真皮包裹', sectionId: 'ergonomic-seats' },
  { name: '隔热玻璃', description: '全车5.35m²玻璃，防晒隔热涂层', sectionId: 'insulated-glass' },
  { name: '电机', description: '21000rpm,加速更快、极速更高、能耗更低，同级领先。', sectionId: 'motor-engine' },
  { name: '电池', description: 'CTB 一体化电池技术,77.8% 全球最高体积效率', sectionId: 'battery' },
  { name: '安全', description: '超过 1050 项安全测试。测试项目数是国家标准的 20 倍，可靠耐久测试时长是国家标准的 96 倍', sectionId: 'safety' },
  { name: '版本', description: 'su7,su7 Pro,su7 Max', sectionId: 'versions' },
  { name: '试驾', description: '前往小米汽车官网进行试驾与选配', sectionId: 'pre-driving' },
  { name: '更多', description: '敬请期待~', sectionId: 'More' },
  { name: '汽车颜色', description: '小米su7拥有9种颜色：海湾蓝、橄榄绿、雅灰、霞光紫、璀璨洋红、寒武岩灰、熔岩橙、珍珠白、钻石黑', sectionId: 'car-color' },
  { name: '内饰颜色', description: '小米su7内饰拥有四种：迷雾紫、暮光红、银河灰、曜石黑、米灰色', sectionId: 'inside-color' },
  { name: '外饰', description: '优雅前脸、饱满尾部、流畅侧面', sectionId: 'outside-page' },
];

function validateSearch(query) {
  if (!query || query.trim().length < 2) {
    return { valid: false, message: '请输入至少2个字符的搜索词' };
  }
  return { valid: true };
}

function performSearch() {
  searchResults.value = [];
  showSearchResults.value = true;
  const validation = validateSearch(searchQuery.value);
  if (!validation.valid) {
    searchError.value = validation.message;
    return;
  }
  searchError.value = '';
  searchResults.value = su7Features.filter(
    feature =>
      feature.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      feature.description.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
  if (searchResults.value.length === 0) {
    searchError.value = '未找到相关结果';
  }
}

function clearSearchResults() {
  if (!searchQuery.value) {
    showSearchResults.value = false;
    searchResults.value = [];
    searchError.value = '';
  }
}

function navigateToSection(sectionId) {
  activeSection.value = 'su7';
  const targetBlock = document.getElementById(sectionId);
  if (targetBlock) {
    targetBlock.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
  showSearchResults.value = false;
}

// Navigate to music section and restart audio
function navigateToMusic() {
  activeSection.value = 'music';
  const musicSection = document.getElementById('music');
  if (musicSection) {
    musicSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
  if (audio.value) {
    audio.value.currentTime = 0; // Reset audio to start
    audio.value.play().catch(err => console.error('Audio play failed:', err));
    isPlaying.value = true;
  }
}

// Navigate back to main section and stop audio
function navigateToMain() {
  activeSection.value = 'su7';
  const mainSection = document.getElementById('su7');
  if (mainSection) {
    mainSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
  if (audio.value) {
    audio.value.pause(); // Stop audio
    audio.value.currentTime = 0; // Reset to start for next play
    isPlaying.value = false;
  }
}

// Video error handling
const videoError = ref(false);
const video = ref(null);

function handleVideoError() {
  console.error('Failed to load video:', video.value?.src);
  videoError.value = true;
}

// Audio handling
const audio = ref(null);
const isPlaying = ref(false);

function toggleAudio() {
  if (audio.value.paused) {
    audio.value.play().catch(err => console.error('Audio play failed:', err));
    isPlaying.value = true;
  } else {
    audio.value.pause();
    isPlaying.value = false;
  }
}

// Section handling
const activeSection = ref('su7');

// Carousel for car colors
const carColors = ref([
  { name: '海湾蓝', src: '/picture/su7_2_1.jpg' },
  { name: '橄榄绿', src: '/picture/su7_2_2.jpg' },
  { name: '熔岩橙', src: '/picture/su7_2_3.jpg' },
  { name: '雅灰', src: '/picture/su7_2_4.jpg' },
  { name: '霞光紫', src: '/picture/su7_2_5.jpg' },
  { name: '寒武岩灰', src: '/picture/su7_2_6.jpg' },
  { name: '璀璨洋红', src: '/picture/su7_2_7.jpg' },
  { name: '珍珠白', src: '/picture/su7_2_8.jpg' },
  { name: '钻石黑', src: '/picture/su7_2_9.jpg' },
]);

const carColorIndex = ref(0);
const showCarColorName = ref(false);
const isCarColorTransitioning = ref(false);
let carColorInterval = null;

function goToCarColor(index) {
  if (isCarColorTransitioning.value) return;
  isCarColorTransitioning.value = true;
  carColorIndex.value = (index + carColors.value.length) % carColors.value.length;
  showCarColorName.value = true;
  setTimeout(() => {
    showCarColorName.value = false;
    setTimeout(() => {
      showCarColorName.value = true;
      isCarColorTransitioning.value = false;
    }, 50);
  }, 300);
}

function nextCarColor() {
  stopCarColorAutoPlay();
  goToCarColor(carColorIndex.value + 1);
  startCarColorAutoPlay();
}

function prevCarColor() {
  stopCarColorAutoPlay();
  goToCarColor(carColorIndex.value - 1);
  startCarColorAutoPlay();
}

function startCarColorAutoPlay() {
  carColorInterval = setInterval(() => nextCarColor(), 5000);
}

function stopCarColorAutoPlay() {
  clearInterval(carColorInterval);
}

// Carousel for inside colors
const insideColors = ref([
  { name: '迷雾紫', src: '/picture/su7_9_1.jpg' },
  { name: '暮光红', src: '/picture/su7_9_2.jpg' },
  { name: '银河灰', src: '/picture/su7_9_3.jpg' },
  { name: '曜石黑', src: '/picture/su7_9_4.jpg' },
  { name: '米灰色', src: '/picture/su7_9_5.jpg' },
]);

const insideColorIndex = ref(0);
const showInsideColorName = ref(false);
const isInsideColorTransitioning = ref(false);
let insideColorInterval = null;

function goToInsideColor(index) {
  if (isInsideColorTransitioning.value) return;
  isInsideColorTransitioning.value = true;
  insideColorIndex.value = (index + insideColors.value.length) % insideColors.value.length;
  showInsideColorName.value = true;
  setTimeout(() => {
    showInsideColorName.value = false;
    setTimeout(() => {
      showInsideColorName.value = true;
      isInsideColorTransitioning.value = false;
    }, 50);
  }, 300);
}

function nextInsideColor() {
  stopInsideColorAutoPlay();
  goToInsideColor(insideColorIndex.value + 1);
  startInsideColorAutoPlay();
}

function prevInsideColor() {
  stopInsideColorAutoPlay();
  goToInsideColor(insideColorIndex.value - 1);
  startInsideColorAutoPlay();
}

function startInsideColorAutoPlay() {
  insideColorInterval = setInterval(() => nextInsideColor(), 5000);
}

function stopInsideColorAutoPlay() {
  clearInterval(insideColorInterval);
}

onMounted(() => {
  startCarColorAutoPlay();
  startInsideColorAutoPlay();
  showCarColorName.value = true;
  showInsideColorName.value = true;
});

onUnmounted(() => {
  stopCarColorAutoPlay();
  stopInsideColorAutoPlay();
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: black;
  color: #FFFFFF;
}

.top-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 50px;
  background-color: auto;
  box-shadow: 0 2px 5px auto;
  position: fixed;
  width: 100%;
  top: 0;
  z-index: 1000;
  height: 70px;
}

.nav-logo-container {
  display: flex;
  align-items: center;
  gap: 15px;
}

.nav-logo {
  cursor: pointer;
}

.nav-logo img {
  height: 40px;
  max-width: 150px;
  object-fit: contain;
}

.back-button {
  background: transparent;
  color: white;
  border: 2px solid white;
  border-radius: 8px;
  padding: 8px 15px;
  font-family: Arial, sans-serif;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.nav-menu {
  display: flex;
  gap: 30px;
  align-items: center;
}

.search-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

.search-input {
  padding: 8px 15px;
  border: 2px solid white;
  border-radius: 8px;
  background: transparent;
  color: white;
  font-family: Arial, sans-serif;
  font-size: 16px;
  width: 200px;
}

.search-input:focus {
  outline: none;
  border-color: orangered;
}

.search-button {
  background: transparent;
  color: white;
  border: 2px solid white;
  border-radius: 8px;
  padding: 8px 15px;
  font-family: Arial, sans-serif;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.3s, transform 0.2s;
}

.search-button:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.search-results {
  position: absolute;
  top: 70px;
  right: 50px;
  background: rgba(0, 0, 0, 0.95);
  border-radius: 8px;
  padding: 15px;
  max-width: 300px;
  display: none;
  z-index: 1000;
}

.search-results.active {
  display: block;
}

.search-results p {
  font-family: Arial, sans-serif;
  font-size: 16px;
  margin: 5px 0;
  color: white;
  cursor: pointer;
  transition: color 0.3s;
}

.search-results p:hover {
  color: orangered;
}

.search-error {
  color: orangered;
  font-size: 14px;
}

.content-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: calc(100vh - 70px);
  padding: 40px 20px;
  max-width: 1200px;
  margin: 0 auto;
  gap: 60px;
  color: #FFFFFF;
}

.content-section {
  display: none;
  width: 100%;
  max-width: 1400px;
}

.content-section.active {
  display: block;
}

.content-block {
  width: 100%;
  max-width: 1400px;
  margin: 60px 0;
  padding: 20px;
  background: rgba(0, 0, 0, 0.9);
  border-radius: 12px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.image-block {
  width: 100%;
  max-width: 1400px;
}

.image-block img {
  width: 100%;
  height: auto;
  max-height: 80vh;
  object-fit: contain;
  border-radius: 12px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.video-block {
  position: relative;
  width: 100%;
  max-width: 1400px;
  aspect-ratio: 16 / 9;
}

.video-block video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 12px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  display: block;
}

.video-error {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  background: transparent;
  padding: 10px 20px;
  border-radius: 8px;
  font-family: Arial, sans-serif;
  font-size: 18px;
  text-align: center;
  z-index: 10;
}

.carousel-container {
  position: relative;
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 12px;
}

.carousel-track {
  display: flex;
  width: 100%;
  transition: transform 0.6s ease;
}

.carousel-item {
  flex: 0 0 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.carousel-item img {
  width: 100%;
  height: auto;
  max-height: 80vh;
  object-fit: contain;
  display: block;
}

.color-name,
.feature-name {
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  color: white;
  font-family: Arial, sans-serif;
  font-size: 24px;
  font-weight: bold;
  background: transparent;
  padding: 10px 20px;
  border-radius: 8px;
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: 10;
}

.color-name.active,
.feature-name.active {
  opacity: 1;
}

.carousel-indicators {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
}

.carousel-indicators span {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.3s;
}

.carousel-indicators span.active {
  background: orangered;
  transform: scale(1.2);
}

.carousel-prev,
.carousel-next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 15px;
  cursor: pointer;
  font-size: 24px;
  transition: all 0.3s;
}

.carousel-prev:hover,
.carousel-next:hover {
  background: transparent;
}

.carousel-prev {
  left: 20px;
}

.carousel-next {
  right: 20px;
}

.text-block {
  border: 2px solid transparent;
  padding: 20px;
  background: transparent;
  border-radius: 8px;
  font-family: Arial, sans-serif;
  font-size: 18px;
  line-height: 1.6;
  color: #FFFFFF;
  text-align: center;
}

.image-display-container {
  display: flex;
  width: 100%;
  gap: 20px;
  height: 60vh;
}

.image-display-left {
  flex: 1;
  width: 50%;
}

.image-display-right {
  flex: 1;
  width: 50%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.image-display-left img,
.image-display-right img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 8px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.image-display-right img {
  height: calc(50% - 10px);
}

.dual-image-display-container {
  display: flex;
  width: 100%;
  gap: 20px;
  height: 60vh;
}

.dual-image-display-left,
.dual-image-display-right {
  flex: 1;
  width: 50%;
}

.dual-image-display-left img,
.dual-image-display-right img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 8px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.three-image-display-container {
  display: flex;
  width: 100%;
  gap: 20px;
  height: 60vh;
  justify-content: center;
  align-items: center;
}

.three-image-item {
  flex: 1;
  max-width: 32%;
  transition: transform 0.3s ease, box-shadow 0.3s ease, opacity 0.3s ease;
  opacity: 0.8;
  cursor: pointer;
}

.three-image-item img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 8px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.three-image-display-container:hover .three-image-item {
  opacity: 0.8;
}

.three-image-item:hover {
  transform: scale(1.1);
  box-shadow: 0 10px 20px rgba(255, 69, 0, 0.5);
  opacity: 1;
  z-index: 1;
}

.action-image-container {
  position: relative;
  width: 100%;
  max-width: 1400px;
}

.button-container {
  position: absolute;
  top: 90%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  gap: 20px;
  z-index: 10;
}

.reserve-button,
.config-button {
  background: transparent;
  color: #FFFFFF;
  font-family: Arial, sans-serif;
  font-size: 24px;
  font-weight: bold;
  padding: 15px 30px;
  border-radius: 8px;
  text-decoration: none;
  transition: background 0.3s, transform 0.2s;
}

.reserve-button:hover,
.config-button:hover {
  background: transparent;
  transform: scale(1.1);
}

.single-button-container {
  display: flex;
  justify-content: center;
  width: 100%;
  max-width: 1400px;
  margin: 40px 0;
  gap: 20px;
}

.more-button {
  background: transparent;
  color: white;
  font-family: Arial, sans-serif;
  font-size: 24px;
  font-weight: bold;
  padding: 15px 30px;
  border: 2px solid white;
  border-radius: 8px;
  text-decoration: none;
  transition: background 0.3s, transform 0.2s;
  cursor: pointer;
}

.more-button:hover {
  background: transparent;
  transform: scale(1.1);
}

.music-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  text-align: center;
}

.music-content img {
  width: 100%;
  max-width: 800px;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.music-content p {
  font-family: Arial, sans-serif;
  font-size: 18px;
  line-height: 1.6;
  max-width: 800px;
}

@media (max-width: 768px) {
  .content-container {
    min-height: calc(100vh - 60px);
    padding: 20px 10px;
  }

  .image-block img,
  .video-block video {
    max-height: 60vh;
    border-radius: 8px;
  }

  .color-name,
  .feature-name {
    font-size: 18px;
    padding: 8px 15px;
  }

  .carousel-indicators span {
    width: 8px;
    height: 8px;
  }

  .carousel-prev,
  .carousel-next {
    padding: 10px;
    font-size: 18px;
  }

  .image-display-container {
    flex-direction: column;
    height: auto;
  }

  .image-display-left,
  .image-display-right {
    width: 100%;
  }

  .image-display-left img {
    height: 30vh;
  }

  .image-display-right img {
    height: 30vh;
  }

  .dual-image-display-container {
    flex-direction: column;
    height: auto;
  }

  .dual-image-display-left,
  .dual-image-display-right {
    width: 100%;
  }

  .dual-image-display-left img,
  .dual-image-display-right img {
    height: 30vh;
  }

  .three-image-display-container {
    flex-direction: column;
    height: auto;
    gap: 10px;
  }

  .three-image-item {
    max-width: 100%;
    height: 30vh;
    opacity: 1;
  }

  .three-image-item:hover {
    transform: scale(1.05);
    box-shadow: 0 10px 20px rgba(255, 69, 0, 0.5);
  }

  .three-image-display-container:hover .three-image-item {
    opacity: 1;
  }

  .button-container {
    flex-direction: column;
    gap: 15px;
  }

  .reserve-button,
  .config-button {
    font-size: 18px;
    padding: 10px 20px;
  }

  .single-button-container {
    flex-direction: column;
    gap: 15px;
  }

  .more-button {
    font-size: 18px;
    padding: 10px 20px;
  }

  .search-container {
    flex-direction: column;
    gap: 10px;
  }

  .search-input {
    width: 100%;
    font-size: 14px;
  }

  .search-button {
    font-size: 14px;
    padding: 8px 15px;
  }

  .search-results {
    right: 10px;
    max-width: 90%;
  }

  .nav-logo-container {
    flex-direction: column;
    gap: 10px;
  }

  .back-button {
    font-size: 14px;
    padding: 8px 12px;
  }
}
</style>
```