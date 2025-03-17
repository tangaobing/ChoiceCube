# 决策魔方应用图片资源

本目录包含决策魔方应用所需的所有图片资源，按以下结构组织：

## 目录结构

```
images/
├── logo/                  # 应用图标和标志
│   └── 3d_cube_logo.png   # 3D魔方图标
│
├── backgrounds/           # 通用背景图片
│   └── abstract_decision_bg.jpg  # 首页抽象背景
│
├── themes/                # 主题相关图片
│   ├── destiny/           # 命运胶囊主题
│   │   ├── blue_tech_particles.jpg  # 主题背景
│   │   └── animation/     # 动画帧
│   │       ├── frame1.jpg
│   │       ├── frame2.jpg
│   │       ├── frame3.jpg
│   │       ├── frame4.jpg
│   │       └── frame5.jpg
│   │
│   ├── divine/            # 神选时刻主题
│   │   ├── ancient_scroll.jpg      # 羊皮纸背景
│   │   ├── golden_magical_pattern.jpg  # 金色神秘背景
│   │   └── animation/     # 动画帧
│   │       ├── frame1.jpg
│   │       ├── frame2.jpg
│   │       ├── frame3.jpg
│   │       ├── frame4.jpg
│   │       └── frame5.jpg
│   │
│   ├── fortune/           # 天机轮主题
│   │   ├── chinese_ink_painting.jpg  # 水墨画背景
│   │   ├── oriental_paper_texture.jpg  # 宣纸纹理
│   │   ├── bagua_symbol.png  # 八卦符号
│   │   └── animation/     # 动画帧
│   │       ├── frame1.jpg
│   │       ├── frame2.jpg
│   │       ├── frame3.jpg
│   │       ├── frame4.jpg
│   │       └── frame5.jpg
│   │
│   └── bubble/            # 气运池主题
│       ├── water_ripple_effect.jpg  # 水波纹背景
│       └── animation/     # 动画帧
│           ├── frame1.jpg
│           ├── frame2.jpg
│           ├── frame3.jpg
│           ├── frame4.jpg
│           └── frame5.jpg
│
└── share/                 # 分享模板
    ├── share_destiny.jpg  # 命运胶囊分享模板
    ├── share_divine.jpg   # 神选时刻分享模板
    ├── share_fortune.jpg  # 天机轮分享模板
    └── share_bubble.jpg   # 气运池分享模板
```

## 图片来源

所有图片均来自免费图片网站，如Unsplash和Pexels，或者是专门为本应用设计的原创图片。

## 图片处理

- 所有背景图片已经过适当的暗化和模糊处理，以确保前景内容清晰可读
- 所有图片已裁剪为合适的尺寸和比例
- 所有图片已添加适当的圆角和阴影效果
- 主题图片已应用相应的滤镜，以符合主题的色调和风格

## 使用说明

在HTML文件中引用图片时，请使用相对路径，例如：

```html
<img src="images/logo/3d_cube_logo.png" alt="决策魔方3D图标">
```

并建议添加`loading="lazy"`属性以优化加载性能：

```html
<img src="images/logo/3d_cube_logo.png" alt="决策魔方3D图标" loading="lazy">
``` 