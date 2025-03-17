# 「决策魔方」产品规格说明书

**产品名称**：决策魔方（ChoiceCube）
 **产品定位**：轻量级决策辅助工具
 **技术要求**：跨平台应用（Android优先），无后台服务依赖

## 一、产品概述

「决策魔方」是一款帮助用户解决日常决策困难的工具应用。用户输入多个选项后，应用通过随机算法和富有趣味的视觉动画体验，为用户提供一个决策建议。产品特色在于提供多种主题化的决策体验，满足用户不同场景和情绪状态下的决策需求。

## 二、功能架构

### 1. 核心功能结构

Plain Text

```plain
1首页
2 ├── 新建决策
3 │    ├── 输入选项
4 │    ├── 选择/分配主题
5 │    ├── 动画展示
6 │    └── 结果呈现
7 └── 历史记录
8      └── 查看/编辑/分享
9
```

### 2. 主题系统

产品提供四种主题风格，各具独特视觉和交互体验：

- **命运胶囊**：科技风格，使用胶囊、数据可视化元素
- **神选时刻**：神圣风格，使用光效、经文等元素
- **天机轮**：东方玄学风格，使用八卦、水墨等元素
- **气运池**：流体风格，使用水波纹、气泡等元素

## 三、页面流程与交互设计

### 1. 首页

**视觉呈现**：

- 中央悬浮的3D魔方（或简化的2D魔方图标）
- 背景为低饱和度渐变色，带有缓慢流动的粒子效果
- 底部"新建决策"主按钮和"历史记录"次级入口

**交互方式**：

- 点击中央魔方可预览不同主题颜色
- 长按魔方可直接进入随机主题的决策流程
- 点击"新建决策"进入选项输入页面
- 首次启动时展示简易引导蒙层（最多3步）

**可行性优化**：

- 魔方动画使用简单的CSS旋转而非真实3D渲染
- 背景粒子数量严格限制（低端设备<50个，高端设备<200个）
- 提供无动画模式开关

### 2. 选项输入页

**视觉呈现**：

- 顶部导航栏包含返回按钮与"继续"按钮
- 中央为输入区域，已输入选项显示为卡片列表
- 底部为输入框与添加按钮

**交互方式**：

- 点击"+"或回车添加新选项
- 左滑选项卡片显示删除按钮
- 长按选项卡片可拖动排序
- 输入≥2个选项后激活"继续"按钮
- 最多支持输入30个选项，超出提示精简

**可行性优化**：

- 使用系统原生输入法，不开发自定义输入键盘
- 选项排序使用简单的拖拽实现，无需复杂动画
- 实时防重复检测使用简单字符串比对算法

### 3. 主题选择页

**视觉呈现**：

- 展示四种主题的预览卡片，带有标题和简短说明
- 当前推荐主题卡片高亮显示
- 底部"开始决策"按钮

**交互方式**：

- 水平滑动浏览不同主题
- 点击任意主题卡片进行选择
- 系统根据选项内容和关键词智能推荐主题
- 点击"开始决策"进入动画页

**主题分配逻辑**：

- 命运胶囊：选项数量≥6时优先推荐
- 神选时刻：选项包含情绪词（如"纠结"、"痛苦"）时推荐
- 天机轮：固定7%的随机概率推荐
- 气运池：默认推荐主题

**可行性优化**：

- 主题预览使用静态图片而非实时动画
- 简化主题推荐算法，使用关键词匹配而非NLP分析

### 4. 动画展示页

**视觉呈现**：

- 全屏动画效果，每个主题具有独特视觉风格
- 顶部显示微小的进度指示器
- 可选的"跳过"按钮（右上角）

**主题动画特性**：

1. **命运胶囊**
   - 动画时长：5秒
   - 视觉元素：透明胶囊容器，内部选项以数据方块形式旋转
   - 阶段划分：胶囊充能→内部震荡→结果突出
   - 音效：科技合成音
2. **神选时刻**
   - 动画时长：7秒
   - 视觉元素：光线从上方照射，选项以经文形式盘旋
   - 阶段划分：光芒汇聚→选项升空→金光突显结果
   - 音效：管风琴和弦
3. **天机轮**
   - 动画时长：6秒
   - 视觉元素：八卦轮盘旋转，选项化为铜钱
   - 阶段划分：轮盘启动→卦象变换→结果定爻
   - 音效：古钟声
4. **气运池**
   - 动画时长：4秒
   - 视觉元素：水面波纹，选项化为气泡
   - 阶段划分：气泡浮现→相互碰撞→胜出气泡上浮
   - 音效：水滴声

**交互方式**：

- 动画过程中点击屏幕可加速动画过程
- 在特定阶段可通过轻摇设备增加随机性
- 动画结束自动跳转至结果页

**可行性保障**：

- 每个主题提供三级动画复杂度，根据设备性能自动匹配
- 低端设备使用简化动画效果（关闭粒子、降低帧率）
- 所有动画可预加载到本地，无网络依赖

### 5. 结果页

**视觉呈现**：

- 与前序动画风格匹配的结果页面
- 中央大号字体显示结果选项
- 底部操作按钮：保存、分享、重新决策
- 可选择显示所有参与选项的列表

**交互方式**：

- 双击结果文字可重放决策动画（0.5倍速）
- 点击分享按钮生成分享卡片（含主题元素和结果）
- 上滑屏幕显示其他选项及其概率权重
- 长按结果可将此次决策保存到历史记录

**可行性保障**：

- 分享功能使用系统原生分享接口
- 分享图片为预设模板+动态文字，非实时渲染
- 历史记录使用轻量级本地数据库存储

### 6. 历史记录页

**视觉呈现**：

- 时间线形式展示历史决策
- 每条记录显示：决策主题、结果、时间、使用的主题
- 顶部搜索栏

**交互方式**：

- 点击记录可查看详情
- 左滑记录可删除
- 按时间或主题进行排序
- 支持搜索历史决策内容

**可行性保障**：

- 限制最大存储记录数（如100条）
- 采用分页加载机制，避免一次性加载过多数据

## 四、主题系统详细设计

### 1. 命运胶囊主题（科技风）

**关键视觉元素**：

- 色调：蓝色系（#0050FF至#00BFFF）
- 材质：半透明玻璃、金属质感、发光边缘
- 动效：电子粒子流、数据雨、全息投影效果

**核心交互体验**：

- 输入选项形成数据胶囊
- 动画过程模拟量子计算和数据处理
- 结果以"命运密码"形式呈现

**适用场景**：选项较多、理性决策场景

**可行性考量**：

- 粒子效果数量严格控制在100以内
- 发光效果使用CSS滤镜而非实时光效
- 3D胶囊使用2.5D效果模拟

### 2. 神选时刻主题（神圣风）

**关键视觉元素**：

- 色调：金色和紫色（#FFD700、#8A2BE2）
- 材质：羊皮纸、古籍、烫金文字
- 动效：光芒四射、羽毛飘落、火焰燃烧

**核心交互体验**：

- 选项呈现为古老经文
- 动画过程模拟神圣仪式和天堂启示
- 结果以"神谕"形式呈现

**适用场景**：情感困扰、重大决定场景

**可行性考量**：

- 光芒效果使用简单放射渐变实现
- 羽毛数量限制在10-20个
- 火焰效果使用循环GIF动画

### 3. 天机轮主题（东方玄学风）

**关键视觉元素**：

- 色调：黑白为主，点缀朱红（#000000、#FFFFFF、#C41E3A）
- 材质：宣纸、墨迹、朱砂
- 动效：水墨晕染、云雾流动、卦象变化

**核心交互体验**：

- 选项转化为八卦符号
- 动画过程模拟周易演算
- 结果以"卦象"形式呈现

**适用场景**：深思熟虑、哲学思考场景

**可行性考量**：

- 水墨效果使用预渲染图片序列
- 云雾使用半透明PNG循环移动
- 卦象使用SVG图形而非复杂渲染

### 4. 气运池主题（流体风）

**关键视觉元素**：

- 色调：湖蓝和水绿（#4FAFD9、#8CDDCD）
- 材质：水面、气泡、透明玻璃
- 动效：水波纹、浮力上升、液体碰撞

**核心交互体验**：

- 选项变为水中气泡
- 动画过程模拟流体物理碰撞
- 结果以"浮出水面"的形式呈现

**适用场景**：轻松休闲、日常小决策场景

**可行性考量**：

- 水波纹使用简化2D波函数计算
- 气泡最多同时显示20个
- 物理碰撞使用简化算法模拟

## 五、技术要点

### 1. 随机算法实现

**基础实现**：

- 使用系统时间毫秒值作为基础种子
- 结合设备ID等固定信息增加随机性
- 可选择使用设备传感器数据（如加速度计）进一步增强随机性

**优化措施**：

- 使用Fisher-Yates洗牌算法确保公平性
- 提供降级方案，当传感器不可用时回退到基础随机
- 每次生成保留种子值，用于结果溯源和重现

### 2. 动画系统

**技术选型**：

- 基础动画：使用平台原生动画系统
- 复杂效果：使用Lottie框架加载JSON动画
- 特殊效果：针对高端设备可使用着色器效果

**性能分级**：

| 设备等级 | 动画复杂度 | 帧率  | 特效           |
| -------- | ---------- | ----- | -------------- |
| 低端设备 | 简化版     | 30fps | 关闭粒子和光效 |
| 中端设备 | 标准版     | 45fps | 保留基础粒子   |
| 高端设备 | 完整版     | 60fps | 全部特效开启   |

**资源控制**：

- 单个主题动画资源不超过3MB
- 优先使用矢量动画减少文件体积
- 实现资源按需加载机制

### 3. 本地数据存储

**存储设计**：

- 使用轻量级键值对数据库（如Hive）
- 历史记录包含：决策ID、标题、选项列表、结果、时间戳、主题类型
- 支持数据导出和备份功能

**性能优化**：

- 限制最大历史记录数量（可配置，默认100条）
- 历史页面采用虚拟列表加载机制
- 定期清理临时缓存数据

## 六、可行性保障措施

### 1. 设备兼容性

**最低设备要求**：

- Android 6.0+, iOS 11.0+
- RAM：至少1.5GB
- 存储空间：应用安装后不超过50MB

**自适应策略**：

- 首次启动进行设备性能检测，设置适配参数
- 提供"性能模式"开关，允许用户手动调节
- 不同分辨率屏幕的自适应布局

### 2. 功能降级方案

**网络不可用**：

- 完全离线运行，无网络依赖
- 分享功能在离线状态下仅生成本地图片

**低端设备**：

- 动画简化为基础过渡效果
- 背景特效全部关闭
- 使用静态图片替代动态元素

**异常情况**：

- 随机算法异常：回退到系统Random
- 动画加载失败：直接显示结果页
- 存储错误：使用内存缓存

### 3. 开发优先级建议

**核心功能（第一阶段）**：

- 基础UI框架和页面导航
- 选项输入和随机选择算法
- 结果展示和历史记录

**增强功能（第二阶段）**：

- 命运胶囊主题完整实现
- 主题切换机制
- 分享功能

**扩展功能（第三阶段）**：

- 其余三个主题实现
- 动画效果优化
- 高级交互体验

## 七、开发资源预估

**开发团队配置**：

- 1名UI/UX设计师
- 2名移动开发工程师
- 1名QA工程师

**开发周期**：

- 第一阶段：2-3周
- 第二阶段：2-3周
- 第三阶段：3-4周
- 总体时间：7-10周

**资源需求**：

- 动画素材：每个主题3-5组Lottie动画
- UI资源：约50个图标和20个背景素材
- 音效资源：约30个交互音效

## 总结

本产品文档提供了「决策魔方」应用的详细功能描述和交互设计，同时兼顾了开发可行性。通过精简视觉效果复杂度、提供降级方案和分阶段实施策略，确保了产品可以在合理开发周期内完成，并在各类设备上提供良好的用户体验。

文档重点描述了四种主题的视觉风格和交互特性，同时提供了具体的技术实现参考，为开发团队提供清晰的产品目标和实施路径。

## 附加内容

```
实施建议:
推荐技术栈:
开发语言：Kotlin
实现方案：Jetpack Compose（更适合复杂动画实现）
动画实现：
    Lottie：加载JSON格式动画资源
    Android动画API：属性动画、过渡动画
    Canvas/自定义View：特殊效果绘制
    Cursor可提供这些API的使用示例
存储方案：
    Room数据库：存储历史记录
    DataStore：应用配置和偏好
    Cursor可生成数据模型和DAO代码
架构模式：
MVVM + 清洁架构
    使用ViewModel、LiveData/Flow
    Cursor可协助生成架构代码
    分阶段实施计划
根据文档的开发优先级建议，可分为三个阶段：
核心功能阶段:
    基础UI框架和页面导航
    选项输入和随机选择算法
    结果展示和历史记录
    Cursor辅助：生成基础布局、导航逻辑和数据模型
增强功能阶段:
    命运胶囊主题完整实现
    主题切换机制
    分享功能
    Cursor辅助：生成动画代码、主题实现逻辑
扩展功能阶段（3-4周）
    其余三个主题实现
    动画效果优化
    高级交互体验
    Cursor辅助：针对复杂动画和交互提供代码示例
四、具体解决方案
	针对产品文档中的几个技术挑战点，提供具体解决方案：
1. 动画系统实现
kotlin
// Cursor可以帮助生成类似以下的动画处理代码
class ThemeAnimationManager(private val context: Context) {
    // 根据设备性能加载不同复杂度的动画
    fun loadAnimation(theme: ThemeType, performanceLevel: PerformanceLevel): LottieAnimationView {
        val animationView = LottieAnimationView(context)
        val animationPath = when(performanceLevel) {
            PerformanceLevel.LOW -> "animations/${theme.name}_simple.json"
            PerformanceLevel.MEDIUM -> "animations/${theme.name}_standard.json"
            PerformanceLevel.HIGH -> "animations/${theme.name}_full.json"
        }
        
        animationView.setAnimation(animationPath)
        animationView.repeatCount = 0
        return animationView
    }
    
    // 监测设备性能
    fun detectDevicePerformance(): PerformanceLevel {
        val activityManager = context.getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager
        val memoryInfo = ActivityManager.MemoryInfo()
        activityManager.getMemoryInfo(memoryInfo)
        
        return when {
            memoryInfo.availMem > 1.5 * 1024 * 1024 * 1024 -> PerformanceLevel.HIGH
            memoryInfo.availMem > 512 * 1024 * 1024 -> PerformanceLevel.MEDIUM
            else -> PerformanceLevel.LOW
        }
    }
}
2. 随机算法实现
kotlin
// Cursor可以帮助生成更精确的随机算法代码
class DecisionEngine {
    // Fisher-Yates洗牌算法
    fun <T> shuffleOptions(options: List<T>): List<T> {
        val random = SecureRandom()
        random.setSeed(System.currentTimeMillis())
        
        val mutableList = options.toMutableList()
        for (i in mutableList.size - 1 downTo 1) {
            val j = random.nextInt(i + 1)
            val temp = mutableList[i]
            mutableList[i] = mutableList[j]
            mutableList[j] = temp
        }
        
        return mutableList
    }
    
    // 使用传感器数据增强随机性
    fun enhanceRandomnessWithSensorData(sensorManager: SensorManager): Long {
        var seed = System.currentTimeMillis()
        val accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
        
        // 加速度传感器数据作为额外随机种子
        if (accelerometer != null) {
            // 实现传感器数据采集
            // ...
        }
        
        return seed
    }
}
3. 本地数据存储
kotlin
// Cursor可以帮助生成完整的数据库架构和DAO代码
@Entity(tableName = "decision_history")
data class DecisionRecord(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val title: String,
    val options: String, // JSON格式存储选项列表
    val result: String,
    val timestamp: Long,
    val themeType: String
)

@Dao
interface DecisionHistoryDao {
    @Query("SELECT * FROM decision_history ORDER BY timestamp DESC LIMIT :limit")
    fun getRecentDecisions(limit: Int): Flow<List<DecisionRecord>>
    
    @Insert
    suspend fun addDecision(record: DecisionRecord)
    
    @Delete
    suspend fun deleteDecision(record: DecisionRecord)
    
    @Query("DELETE FROM decision_history WHERE id IN (SELECT id FROM decision_history ORDER BY timestamp ASC LIMIT :count)")
    suspend fun deleteOldestRecords(count: Int)
}
五、总结与建议
可行性结论
使用Cursor作为辅助工具开发"决策魔方"Android应用是完全可行的。Cursor可以帮助提高开发效率，尤其在生成标准代码、实现算法、构建UI等方面。
实际开发需要结合其他工具，如Android Studio、版本控制等共同完成，Cursor不能替代完整的开发环境，但能显著提升开发效率。
建议
架构选择：采用MVVM+Clean Architecture，便于功能分离和测试
UI框架选择：
如果开发团队熟悉传统Android开发：使用XML布局+MotionLayout
如果追求更现代的开发体验：考虑Jetpack Compose
性能优化预案：
预先设计不同动画复杂度版本
实现资源懒加载机制
采用App Bundle减小安装包体积
```

