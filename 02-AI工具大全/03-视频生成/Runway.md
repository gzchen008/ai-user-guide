# Runway 完全指南

> 专业创作者的 AI 视频工具

---

## 🤔 什么是 Runway？

**Runway = 专业级 AI 视频创作平台**

简单来说，Runway 是一个提供多种 AI 视频和图像工具的平台，被广泛应用于电影、广告、艺术创作等领域。

**Runway 的核心产品：**
- 🎬 **Gen-2** - 文字/图片生成视频
- 🎨 **Gen-1** - 视频风格转换
- 🖼️ **图像工具** - 超分辨率、背景移除等
- 🎥 **实时绿幕** - AI 抠图

**一句话总结：** Runway 是功能最全的专业 AI 视频平台

---

## 🌟 Runway 核心功能

### 1️⃣ Gen-2（视频生成）

**文字生成视频：**
```
Input: "A cat walking in a beautiful garden, sunny day"
Output: 4 秒视频
```

**图片生成视频：**
```
Input: 上传一张图片 + 动作描述
Output: 图片变成动态视频
```

**视频 + 文字：**
```
Input: 上传视频 + "改成水彩风格"
Output: 风格转换后的视频
```

---

### 2️⃣ Gen-1（风格转换）

**功能：** 改变视频风格，保持动作

**示例：**
- 真人视频 → 动漫风格
- 白天 → 夜晚
- 现代服装 → 古装

**应用：**
- 音乐视频制作
- 创意广告
- 艺术实验

---

### 3️⃣ 图像工具

**超分辨率：**
- 720p → 1080p / 4K
- 修复低清视频

**背景移除：**
- 自动抠图
- 绿幕替代

**图像扩展：**
- 扩展图片边界
- 调整构图

---

### 4️⃣ 实时绿幕

**功能：** 无需绿幕背景，AI 实时抠图

**应用：**
- 直播
- 视频会议
- 虚拟背景

---

## 🚀 快速开始

### 1️⃣ 注册账号

**步骤：**
1. 访问 [runwayml.com](https://runwayml.com)
2. 注册账号（可用 Google 登录）
3. 获得免费额度（125 秒视频）

---

### 2️⃣ 生成第一个视频

**文字生成视频：**

1. 点击 "Gen-2"
2. 选择 "Text to Video"
3. 输入提示词：
   ```
   A beautiful sunset over the ocean, waves gently rolling, golden light, cinematic
   ```
4. 点击 "Generate"
5. 等待 1-2 分钟
6. 下载视频

---

### 3️⃣ 图片生成视频

**步骤：**
1. 选择 "Image to Video"
2. 上传图片
3. 选择运动方式：
   - `Camera Motion` - 镜头运动
   - `Motion Brush` - 局部运动
4. 调整参数
5. 生成视频

---

## 🎨 Gen-2 参数详解

### 1️⃣ Motion Score（运动强度）

**范围：** 1-10

**建议：**
- **1-3** - 轻微运动（适合风景）
- **4-6** - 中等运动（通用）
- **7-10** - 强烈运动（动作场景）

---

### 2️⃣ Seed（随机种子）

**作用：** 相同 seed 生成相同视频

**用法：**
- 满意的视频，记下 seed
- 可以微调参数重新生成
- 保持画面一致性

---

### 3️⃣ Motion Brush（运动画笔）

**功能：** 选择视频中的特定区域运动

**步骤：**
1. 上传图片
2. 用画笔标记要运动的区域
3. 选择运动方向
4. 生成视频

**应用：**
- 只让水流动
- 只让云飘动
- 人物局部动作

---

### 4️⃣ Camera Motion（镜头运动）

**选项：**
- `Horizontal` - 左右移动
- `Vertical` - 上下移动
- `Zoom` - 推拉镜头
- `Pan` - 摇镜头
- `Tilt` - 俯仰镜头

---

## 💡 实战案例

### 案例1：风景延时

**Prompt：**
```
Time-lapse of mountains at sunrise, clouds moving rapidly, golden light spreading across peaks, cinematic, 4K
```

**设置：**
- Motion Score: 5
- Camera: Slow zoom in
- Duration: 4 秒

---

### 案例2：人物肖像

**Prompt：**
```
Portrait of a young woman, soft smile, natural lighting, slight head movement, cinematic depth of field
```

**设置：**
- Motion Score: 2（轻微运动）
- Camera: Static
- Duration: 4 秒

---

### 案例3：产品展示

**Prompt：**
```
Product shot of a luxury watch, rotating slowly, studio lighting, white background, professional commercial
```

**设置：**
- Motion Score: 3
- Camera: Rotate
- Duration: 4 秒

---

### 案例4：科幻场景

**Prompt：**
```
Futuristic city with flying cars, neon lights, rain, cyberpunk atmosphere, Blade Runner style
```

**设置：**
- Motion Score: 7
- Camera: Aerial tracking
- Duration: 4 秒

---

## 📊 Runway 定价

### 免费版

| 项目 | 额度 |
|------|------|
| 视频时长 | 125 秒 |
| 分辨率 | 720p |
| 水印 | ✅ 有 |

---

### 付费计划

| 计划 | 价格 | 视频时长 | 分辨率 | 水印 |
|------|------|---------|--------|------|
| **Standard** | $12/月 | 525 秒 | 1080p | ❌ 无 |
| **Pro** | $28/月 | 2250 秒 | 1080p | ❌ 无 |
| **Unlimited** | $76/月 | 无限 | 1080p | ❌ 无 |

**建议：**
- 新手：免费版（体验）
- 日常使用：Standard
- 重度用户：Pro 或 Unlimited

---

## 🔄 Runway vs 其他工具

| 功能 | Runway Gen-2 | Pika | Sora |
|------|--------------|------|------|
| **时长** | 4 秒 | 3 秒 | 60 秒 |
| **分辨率** | 1080p | 720p | 1080p |
| **风格转换** | ✅ Gen-1 | ❌ | ❌ |
| **工具丰富度** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **公开访问** | ✅ | ✅ | ❌ |
| **价格** | $12/月起 | 免费/付费 | 未公开 |

**选择建议：**
- 追求功能全面 → **Runway**
- 追求免费 → Pika
- 追求长视频 → 等待 Sora

---

## 🔧 高级技巧

### 1️⃣ 多次迭代

**方法：**
1. 生成 4 秒视频
2. 下载最后一帧
3. 用最后一帧作为新的输入
4. 继续生成 4 秒
5. 重复...

**效果：** 可以生成较长的连贯视频

---

### 2️⃣ 结合图片生成

**流程：**
1. 用 MidJourney 生成高质量图片
2. 用 Runway 让图片动起来
3. 获得高质量视频

**优势：** 图片质量更高，视频效果更好

---

### 3️⃣ 风格参考

**方法：**
1. 找到喜欢的视频风格
2. 用 Gen-1 风格转换
3. 应用到你的视频

**示例：**
```
上传普通视频 + "Pixar animation style"
= 皮克斯风格视频
```

---

### 4️⃣ 批量生成

**技巧：**
- 一次生成多个变体
- 选择最好的一个
- 节省时间和成本

---

## 📚 推荐资源

**官方资源：**
- [Runway 官网](https://runwayml.com)
- [Runway YouTube](https://youtube.com/@runwayml)
- [Runway Discord](https://discord.gg/runway)

**教程：**
- [Runway Academy](https://runwayml.com/academy)
- [YouTube 教程](https://youtube.com/results?search_query=runway+gen-2+tutorial)

**社区：**
- Reddit r/runwayml
- Twitter/X: @runwayml

---

## ❓ 常见问题

<details>
<summary><b>Q: Runway 免费额度用完怎么办？</b></summary>

**A: 可以升级或等待。**

- 升级到付费计划
- 每月刷新免费额度
- 或使用其他免费工具（Pika）
</details>

<details>
<summary><b>Q: 生成的视频可以商用吗？</b></summary>

**A: 付费用户可以。**

- 免费版：仅供个人使用
- 付费版：可商用
- 无水印版适合商业项目
</details>

<details>
<summary><b>Q: Runway 和 Sora 选哪个？</b></summary>

**A: 目前只能选 Runway。**

- Sora 未公开
- Runway 是目前最好的选择
- Sora 公开后可以对比
</details>

<details>
<summary><b>Q: 为什么视频只有 4 秒？</b></summary>

**A: 技术限制。**

- AI 视频生成长度有限
- Runway 正在改进
- 可以通过迭代延长
</details>

---

## 🎯 学习路径

**第 1 天：基础使用**
- 注册账号
- 生成第一个视频
- 理解参数

**第 1 周：掌握技巧**
- 尝试不同场景
- 学习 Motion Brush
- 结合图片生成

**第 1 月：专业应用**
- 风格转换
- 批量生成
- 应用于实际项目

---

**💡 小贴士：** Runway 的优势在于"工具丰富"，除了视频生成，还有图像处理、实时绿幕等功能，是专业创作者的全能工具箱！
