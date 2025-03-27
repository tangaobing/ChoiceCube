# 「决策魔方」产品开发规格说明书 - UniApp版

**产品名称**：决策魔方（ChoiceCube）
**产品定位**：轻量级决策辅助工具
**技术框架**：基于UniApp的跨平台应用，支持H5/微信小程序/App

## 一、产品概述

「决策魔方」是一款帮助用户解决日常决策困难的工具应用。用户输入多个选项后，应用通过随机算法和富有趣味的视觉动画体验，为用户提供一个决策建议。产品特色在于提供多种主题化的决策体验，满足用户不同场景和情绪状态下的决策需求。

## 二、技术架构与开发框架

### 1. 开发框架选择

- **主框架**：UniApp（基于Vue.js的跨平台框架）
- **开发工具**：Cursor IDE（提供代码辅助、智能补全等增强功能）
- **基础UI库**：uView UI / uni-ui
- **页面参考**：prototype文件夹下的HTML页面设计

### 2. 功能结构

```
1首页(参考prototype/home.html)
2 ├── 新建决策(参考prototype/input_options.html - 选项输入页，用于输入决策问题和选项)
3 │    ├── 输入选项
4 │    ├── 选择/分配主题(参考prototype/theme_select.html)
5 │    ├── 动画展示(参考prototype/animation_destiny.html、animation_divine.html、animation_fortune.html、animation_bubble.html)
6 │    └── 结果呈现(参考prototype/result.html)
7 └── 历史记录(参考prototype/history.html)
8      └── 查看/编辑/分享(历史详情页)
```

### 3. 主题系统

产品提供四种主题风格，各具独特视觉和交互体验：

- **命运胶囊**：科技风格，使用胶囊、数据可视化元素
- **神选时刻**：神圣风格，使用光效、经文等元素
- **天机轮**：东方玄学风格，使用八卦、水墨等元素
- **气运池**：流体风格，使用水波纹、气泡等元素

## 三、页面流程与交互设计

### 1. 首页

**页面参考**：`prototype/home.html`

**视觉实现**：
- 中央的魔方图标（使用CSS动画实现简单旋转效果）
- 渐变背景（使用CSS渐变实现）
- 简单粒子效果（使用canvas实现，数量低于30个）

**交互方式**：
- 点击中央魔方显示主题预览
- 长按魔方可直接进入随机主题的决策流程
- 点击"新建决策"按钮进入选项输入页面

**UniApp实现关键点**：
```vue
<template>
  <view class="home-container">
    <canvas id="particleCanvas" canvas-id="particleCanvas" class="particle-bg"></canvas>
    <view class="cube-container" @tap="previewThemes" @longpress="startRandomDecision">
      <!-- 使用CSS 3D变换实现简单魔方效果 -->
      <view class="cube" :class="{ 'rotate': isRotating }">
        <view class="cube-face front"></view>
        <view class="cube-face back"></view>
        <view class="cube-face right"></view>
        <view class="cube-face left"></view>
        <view class="cube-face top"></view>
        <view class="cube-face bottom"></view>
      </view>
    </view>
    <view class="btn-container">
      <button class="btn-primary" @tap="navigateToNewDecision">新建决策</button>
      <button class="btn-secondary" @tap="navigateToHistory">历史记录</button>
    </view>
  </view>
</template>
```

**性能优化**：
- 根据平台自动调整粒子数量（H5/小程序平台<20个，App平台<30个）
- 提供无动画模式开关（存储在uni.storage中）
- 使用CSS动画代替复杂JavaScript动画

### 2. 选项输入页

**页面参考**：`prototype/input_options.html`

**视觉实现**：
- 使用UniApp的导航栏组件
- 卡片列表呈现已添加的选项
- 底部固定输入区域

**交互方式**：
- 使用uni-ui的SwipeAction组件实现左滑删除
- 输入框回车或点击添加按钮新增选项
- 最小2个选项，最多30个选项

**UniApp实现关键点**：
```vue
<template>
  <view class="option-page">
    <uni-nav-bar title="添加选项" left-icon="back" @clickLeft="goBack" right-text="继续" :rightText-style="{color: options.length >= 2 ? '#007AFF' : '#999'}" @clickRight="continueIfValid"></uni-nav-bar>
    
    <view class="options-container">
      <uni-swipe-action v-for="(item, index) in options" :key="index">
        <uni-swipe-action-item :right-options="swipeOptions" @click="handleSwipeClick($event, index)">
          <view class="option-item">
            <text>{{item}}</text>
          </view>
        </uni-swipe-action-item>
      </uni-swipe-action>
    </view>
    
    <view class="input-container">
      <input class="option-input" v-model="newOption" placeholder="输入选项内容" @confirm="addOption" />
      <button class="add-btn" @tap="addOption">+</button>
    </view>
  </view>
</template>
```

**存储处理**：
- 使用Vuex管理选项数据
- 使用uni.storage缓存未完成的决策

### 3. 主题选择页

**页面参考**：`prototype/theme_select.html`

**视觉实现**：
- 水平滑动卡片样式
- 主题预览使用静态图片而非动画

**交互方式**：
- uni-swiper组件实现主题卡片滑动
- 点击卡片选择对应主题
- 显示推荐主题标记

**UniApp实现关键点**：
```vue
<template>
  <view class="theme-selection">
    <uni-nav-bar title="选择主题" left-icon="back" @clickLeft="goBack"></uni-nav-bar>
    
    <swiper class="themes-swiper" :indicator-dots="true" :autoplay="false">
      <swiper-item v-for="(theme, index) in themes" :key="index" @tap="selectTheme(theme.id)">
        <view class="theme-card" :class="{'active': selectedTheme === theme.id}">
          <image :src="theme.previewImage" mode="aspectFill"></image>
          <view class="theme-name">{{theme.name}}</view>
          <view class="theme-desc">{{theme.description}}</view>
          <view v-if="theme.recommended" class="recommended-tag">推荐</view>
        </view>
      </swiper-item>
    </swiper>
    
    <button class="start-btn" @tap="startDecision" :disabled="!selectedTheme">开始决策</button>
  </view>
</template>
```

**主题推荐逻辑**：
- 使用简单关键词匹配而非复杂NLP分析
- 基于选项数量和内容进行推荐

### 4. 动画展示页

**页面参考**：`prototype/animation_destiny.html`、`animation_divine.html`、`animation_fortune.html`、`animation_bubble.html`

**视觉实现**：
- 全屏Canvas或使用uni-app组件结合CSS动画
- 进度条指示动画剩余时间
- 右上角跳过按钮

**技术方案**：
- 基础动画使用CSS/Animation.css
- 复杂动画使用uni-app的canvas API
- 粒子效果使用第三方库如particles.js的移植版

**主题动画适配**：
- 针对不同平台提供不同复杂度的动画
- H5/小程序版本使用轻量级CSS动画
- App版本可使用更复杂的canvas动画

**UniApp实现关键点**：
```vue
<template>
  <view class="animation-container">
    <view class="progress-bar">
      <view class="progress" :style="{width: progress + '%'}"></view>
    </view>
    
    <view class="skip-btn" @tap="skipAnimation">跳过</view>
    
    <!-- 命运胶囊主题 -->
    <block v-if="currentTheme === 'capsule'">
      <view class="capsule-container">
        <canvas id="capsuleCanvas" canvas-id="capsuleCanvas" class="theme-canvas"></canvas>
      </view>
    </block>
    
    <!-- 其他主题类似结构 -->
    
    <!-- 使用uni-app提供的API调用设备振动 -->
    <button class="interact-hint" @tap="interactWithAnimation">轻摇设备增加随机性</button>
  </view>
</template>
```

**性能优化**：
- 预加载动画资源（使用uni.downloadFile缓存资源）
- 根据设备性能自动降级（检测帧率表现）
- 简化物理碰撞等复杂效果

### 5. 结果页

**页面参考**：`prototype/result.html`

**视觉实现**：
- 与主题风格一致的结果展示
- 中央突出显示结果
- 底部操作按钮行

**交互方式**：
- 双击结果可重放简化动画
- 使用uni.share实现系统分享功能
- 上滑屏幕显示其他选项

**UniApp实现关键点**：
```vue
<template>
  <view class="result-page" :class="'theme-' + currentTheme">
    <view class="result-container">
      <view class="result-title">决策结果</view>
      <view class="result-text" @tap.double="replayAnimation">{{result}}</view>
    </view>
    
    <view class="action-bar">
      <button class="action-btn" @tap="saveToHistory">保存</button>
      <button class="action-btn" @tap="shareResult">分享</button>
      <button class="action-btn" @tap="newDecision">重新决策</button>
    </view>
    
    <view class="other-options" v-if="showOtherOptions">
      <view class="option-item" v-for="(option, index) in allOptions" :key="index">
        <text>{{option}}</text>
      </view>
    </view>
  </view>
</template>
```

**分享实现**：
- 使用uni.share API实现原生分享
- 使用canvas绘制分享图片（uniapp提供跨平台绘图API）

### 6. 历史记录页

**页面参考**：`prototype/history.html`

**视觉实现**：
- 时间线形式展示历史决策
- 搜索栏置顶
- 支持左滑删除

**交互方式**：
- 点击记录查看详情
- 搜索功能（支持搜索结果和选项）
- 分页加载（每页20条记录）

**UniApp实现关键点**：
```vue
<template>
  <view class="history-page">
    <uni-nav-bar title="历史记录" left-icon="back" @clickLeft="goBack"></uni-nav-bar>
    
    <view class="search-container">
      <input class="search-input" v-model="searchText" placeholder="搜索历史决策" @input="handleSearch" />
    </view>
    
    <view class="history-list">
      <uni-swipe-action v-for="(item, index) in displayHistoryList" :key="index">
        <uni-swipe-action-item :right-options="swipeOptions" @click="handleSwipeClick($event, index)">
          <view class="history-item" @tap="viewDetail(item.id)">
            <view class="history-theme-tag" :class="'theme-' + item.theme">{{getThemeName(item.theme)}}</view>
            <view class="history-result">{{item.result}}</view>
            <view class="history-time">{{formatDate(item.timestamp)}}</view>
          </view>
        </uni-swipe-action-item>
      </uni-swipe-action>
      
      <uni-load-more :status="loadMoreStatus" @clickLoadMore="loadMore"></uni-load-more>
    </view>
  </view>
</template>
```

**数据处理**：
- 使用uni.storage存储历史记录（限制100条）
- 实现简单索引提高搜索效率
- 定期清理过期数据

## 四、主题系统设计与UniApp实现

### 1. 命运胶囊主题（科技风）

**视觉风格**：
- 色调：蓝色系（#0050FF至#00BFFF）
- 材质：半透明玻璃、金属质感

**UniApp实现方案**：
- 使用CSS变量定义主题色
- 使用CSS滤镜模拟发光效果
- 使用SVG或Icon Font替代复杂图形

**动画简化方案**：
- HTML5版本：使用CSS动画+简单Canvas绘制
- App版本：可使用更复杂的Canvas动画
- 降级方案：仅使用CSS过渡效果

**参考原型**：`prototype/animation_destiny.html`

### 2. 神选时刻主题（神圣风）

**视觉风格**：
- 色调：金色和紫色（#FFD700、#8A2BE2）
- 材质：羊皮纸、烫金文字

**UniApp实现方案**：
- 使用WebP格式优化背景图片
- 使用CSS动画模拟光芒效果
- 使用图片序列模拟复杂动画

**动画简化方案**：
- 使用CSS Animation代替Canvas动画
- 羽毛效果使用精灵图实现
- 光效使用CSS径向渐变

**参考原型**：`prototype/animation_divine.html`

### 3. 天机轮主题（东方玄学风）

**视觉风格**：
- 色调：黑白为主，点缀朱红（#000000、#FFFFFF、#C41E3A）
- 材质：宣纸、墨迹

**UniApp实现方案**：
- SVG实现八卦符号
- 静态PNG图片模拟水墨效果
- CSS动画实现旋转效果

**动画简化方案**：
- 使用预渲染的图片序列
- 减少同屏动画元素数量
- 使用CSS transform代替Canvas渲染

**参考原型**：`prototype/animation_fortune.html`

### 4. 气运池主题（流体风）

**视觉风格**：
- 色调：湖蓝和水绿（#4FAFD9、#8CDDCD）
- 材质：水面、气泡

**UniApp实现方案**：
- 使用CSS波浪动画模拟水面
- 使用SVG或PNG实现气泡
- 结合Canvas实现简单流体效果

**动画简化方案**：
- 减少气泡数量（最多10个）
- 使用CSS动画代替物理碰撞模拟
- 预渲染部分效果为GIF动画

**参考原型**：`prototype/animation_bubble.html`

## 五、技术实现要点

### 1. UniApp框架优化

**兼容性处理**：
```js
// 根据平台判断并执行不同实现
const platform = uni.getSystemInfoSync().platform;
const isH5 = process.env.UNI_PLATFORM === 'h5';
const isMP = process.env.UNI_PLATFORM.indexOf('mp-') !== -1;
const isApp = process.env.UNI_PLATFORM === 'app-plus';

// 选择合适的动画实现方式
function initAnimation() {
  if (isH5 || isMP) {
    return initLightAnimation(); // 轻量级动画
  } else if (isApp) {
    return initFullAnimation(); // 完整动画
  }
}
```

**条件编译**：
```js
// 平台差异处理
// #ifdef APP-PLUS
// App 平台特有代码
const vibrate = () => {
  uni.vibrateLong();
};
// #endif

// #ifdef H5 || MP-WEIXIN
// H5和微信小程序平台特有代码
const vibrate = () => {
  // 检查API可用性
  if (uni.vibrateShort) {
    uni.vibrateShort();
  }
};
// #endif
```

### 2. 随机算法实现

**基础随机实现**：
```js
// Cursor可以优化和提供更高效的算法实现
export const DecisionEngine = {
  /**
   * Fisher-Yates 洗牌算法
   * @param {Array} options 选项数组
   * @returns {Array} 打乱后的数组
   */
  shuffleOptions(options) {
    const array = [...options];
    let currentIndex = array.length;
    let randomIndex;
    
    // 当还剩余元素未洗牌时
    while (currentIndex !== 0) {
      // 选择一个剩余元素
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex--;
      
      // 与当前元素交换
      [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
    }
    
    return array;
  },
  
  /**
   * 使用设备传感器数据增强随机性
   * @returns {Promise<number>} 随机种子值
   */
  async enhanceRandomness() {
    let seed = Date.now();
    
    // 尝试获取加速度计数据
    try {
      await new Promise((resolve) => {
        uni.onAccelerometerChange((res) => {
          // 使用加速度数据调整种子
          seed += (res.x * 1000000) + (res.y * 10000) + (res.z * 100);
          uni.offAccelerometerChange();
          resolve();
        });
        uni.startAccelerometer({
          interval: 'game'
        });
        
        // 设置超时，防止无限等待
        setTimeout(() => {
          uni.offAccelerometerChange();
          resolve();
        }, 500);
      });
    } catch (e) {
      console.log('无法访问加速度计，使用默认随机种子');
    }
    
    return seed;
  }
};
```

### 3. 本地数据存储

**存储封装**：
```js
// Cursor可以生成完整的存储管理代码
export const StorageManager = {
  /**
   * 最大历史记录数量
   */
  MAX_HISTORY: 100,
  
  /**
   * 保存决策记录
   * @param {Object} decision 决策记录
   */
  saveDecision(decision) {
    const history = this.getHistory();
    
    // 添加新记录
    history.unshift({
      id: Date.now().toString(),
      title: decision.title || '未命名决策',
      options: decision.options,
      result: decision.result,
      timestamp: Date.now(),
      theme: decision.theme
    });
    
    // 限制记录数量
    if (history.length > this.MAX_HISTORY) {
      history.splice(this.MAX_HISTORY);
    }
    
    // 存储到本地
    uni.setStorageSync('decision_history', JSON.stringify(history));
  },
  
  /**
   * 获取历史记录
   * @returns {Array} 历史记录数组
   */
  getHistory() {
    try {
      const data = uni.getStorageSync('decision_history');
      return data ? JSON.parse(data) : [];
    } catch (e) {
      console.error('读取历史记录失败', e);
      return [];
    }
  },
  
  /**
   * 删除指定记录
   * @param {string} id 记录ID
   */
  deleteDecision(id) {
    const history = this.getHistory();
    const index = history.findIndex(item => item.id === id);
    
    if (index !== -1) {
      history.splice(index, 1);
      uni.setStorageSync('decision_history', JSON.stringify(history));
    }
  }
};
```

## 六、开发与实施计划

### 1. 开发环境配置

**Cursor IDE 优化**：
- 启用Vue文件语法高亮和智能提示
- 配置ESLint规则适应Vue单文件组件
- 使用 Cursor 的 AI 生成功能辅助编写复杂组件

**UniApp项目结构**：
```
decision-cube/
├── src/
│   ├── pages/               # 页面文件
│   │   ├── index/           # 首页
│   │   ├── new-decision/    # 选项输入页
│   │   ├── theme-select/    # 主题选择页
│   │   ├── animation/       # 动画页
│   │   ├── result/          # 结果页
│   │   └── history/         # 历史记录页
│   ├── components/          # 公共组件
│   │   ├── theme-card.vue
│   │   └── option-item.vue
│   ├── static/              # 静态资源
│   │   ├── images/
│   │   ├── themes/
│   │   └── animations/
│   ├── utils/               # 工具类
│   │   ├── decision-engine.js
│   │   └── storage.js
│   ├── store/               # Vuex数据
│   │   └── index.js
│   ├── App.vue              # 应用入口
│   └── main.js              # 主文件
├── prototype/               # HTML原型参考
└── package.json             # 项目配置
```

### 2. 开发阶段规划

**第一阶段（基础功能，2周）**：
- 搭建UniApp框架和基础页面
- 实现选项输入和随机选择算法
- 完成简单的CSS风格主题
- 实现本地存储功能

**第二阶段（增强功能，2周）**：
- 完成命运胶囊主题的动画实现
- 优化页面过渡和交互体验
- 添加分享功能
- 实现跨平台兼容性处理

**第三阶段（完善功能，3周）**：
- 完成剩余三个主题的实现
- 优化动画性能
- 完善历史记录和搜索功能
- 多平台测试与调优

### 3. 技术难点解决方案

**动画性能优化**：
- 使用requestAnimationFrame而非setInterval
- 合理使用CSS硬件加速
- 实现动画帧率监测自动降级

```js
// 动画性能监测与自动降级
export function monitorAnimationPerformance(callback) {
  let lastTime = 0;
  let frames = 0;
  let totalTime = 0;
  
  function checkFrame(timestamp) {
    if (lastTime !== 0) {
      const deltaTime = timestamp - lastTime;
      totalTime += deltaTime;
      frames++;
      
      // 每秒计算一次帧率
      if (totalTime >= 1000) {
        const fps = Math.round((frames * 1000) / totalTime);
        
        // 性能不佳时降级
        if (fps < 30) {
          callback('low');
        } else if (fps < 50) {
          callback('medium');
        } else {
          callback('high');
        }
        
        frames = 0;
        totalTime = 0;
      }
    }
    
    lastTime = timestamp;
    requestAnimationFrame(checkFrame);
  }
  
  requestAnimationFrame(checkFrame);
}
```

**跨平台兼容性**：
- 使用条件编译处理平台差异
- 为不同平台提供不同复杂度的动画实现
- 图片资源按需加载策略

## 七、资源需求估计

**开发团队配置**：
- 1名UI/UX设计师
- 2名前端工程师（熟悉UniApp/Vue.js）
- 1名QA工程师

**开发周期**：
- 第一阶段：2周
- 第二阶段：2周
- 第三阶段：3周
- 总体时间：7周

**资源需求**：
- UI设计稿：Figma/Sketch格式
- 动画素材：每个主题的基础动画效果（CSS/Lottie）
- 音效资源：12个基础交互音效
- 图标资源：约40个SVG格式图标

## 八、总结

本文档将原有的Android产品设计转换为基于UniApp的跨平台实现方案，并根据Cursor IDE的开发特性进行了优化。主要变更包括：

1. 框架转换：从Android原生改为UniApp跨平台框架
2. 参考设计：明确参考prototype文件夹下的HTML原型页面
3. 技术优化：针对Cursor IDE的AI辅助功能提供了更具体的代码示例
4. 性能考量：针对不同平台（H5/小程序/App）提供分级实现方案
5. 开发规划：具体的项目结构和开发阶段规划

该实现方案保留了原有产品的核心功能和用户体验，同时通过UniApp框架实现了跨平台支持，有效降低了开发成本和维护难度。Cursor IDE的AI辅助功能可以显著提高编码效率，特别是在生成模板代码和处理复杂算法方面。