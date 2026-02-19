# MidJourney 完全指南

> 让普通人也能创作出专业级艺术作品

---

## 🤔 什么是 MidJourney？

**MidJourney = AI 图像生成界的"艺术家"**

简单来说，MidJourney 是一个 AI 图像生成工具，你只需要用文字描述想要的画面，它就能生成令人惊叹的艺术作品。

**MidJourney 的核心特点：**
- 🎨 **艺术性强** - 生成质量极高，审美在线
- 💫 **风格多样** - 写实、动漫、抽象、油画...
- ⚡ **速度快** - 几十秒生成 4 张图
- 🔄 **可迭代** - 不断优化直到满意

**一句话总结：** MidJourney 是目前艺术质量最高的 AI 绘图工具

---

## 🆚 MidJourney vs 其他工具

| 工具 | 艺术质量 | 速度 | 易用性 | 价格 |
|------|---------|------|--------|------|
| **MidJourney** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | $10/月起 |
| DALL-E 3 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ChatGPT Plus |
| Stable Diffusion | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | 免费 |
| Ideogram | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 免费/付费 |

**选择建议：**
- 追求艺术质量 → **MidJourney**
- 追求易用性 → DALL-E 3
- 追求免费 → Stable Diffusion / Ideogram

---

## 🚀 快速开始

### 1️⃣ 注册和订阅

**MidJourney 目前只在 Discord 上使用**

**步骤：**

1. **注册 Discord 账号**
   - 访问 [discord.com](https://discord.com)
   - 注册账号（免费）

2. **加入 MidJourney 服务器**
   - 访问 [midjourney.com](https://midjourney.com)
   - 点击 "Join the Beta"
   - 自动跳转到 Discord

3. **订阅付费计划**
   - 在 Discord 中输入 `/subscribe`
   - 选择计划：
     - **Basic**: $10/月 (200张图)
     - **Standard**: $30/月 (无限张)
     - **Pro**: $60/月 (无限张 + 隐私)

---

### 2️⃣ 第一张图

**在任意 #newbies 频道输入：**

```
/imagine prompt: 一只可爱的橘猫坐在窗台上看夕阳，水彩风格
```

**等待 30-60 秒，你会得到：**
- 4 张预览图
- 每张图下面有按钮：U1-U4, V1-V4

---

### 3️⃣ 按钮说明

**U 按钮（Upscale）- 放大：**
- `U1` - 放大第 1 张图（左上）
- `U2` - 放大第 2 张图（右上）
- `U3` - 放大第 3 张图（左下）
- `U4` - 放大第 4 张图（右下）

**V 按钮（Variation）- 变体：**
- `V1` - 基于第 1 张图生成 4 个变体
- `V2` - 基于第 2 张图生成 4 个变体
- `V3` - 基于第 3 张图生成 4 个变体
- `V4` - 基于第 4 张图生成 4 个变体

**其他按钮：**
- `🔄` - 重新生成 4 张图
- `Web` - 在浏览器中打开
- `❤️` - 收藏

---

## 🎨 Prompt 提示词技巧

### 1️⃣ 基础结构

**公式：**
```
主体 + 描述 + 风格 + 参数
```

**示例：**
```
/imagine prompt: 一座未来城市，赛博朋克风格，霓虹灯，夜晚，电影质感 --ar 16:9 --v 6
```

---

### 2️⃣ 常用风格关键词

**艺术风格：**
- `oil painting` - 油画
- `watercolor` - 水彩
- `digital art` - 数字艺术
- `anime` - 动漫
- `photorealistic` - 照片写实
- `abstract` - 抽象
- `impressionism` - 印象派
- `minimalist` - 极简主义

**光照效果：**
- `golden hour` - 黄金时刻（日落）
- `blue hour` - 蓝调时刻（黄昏）
- `studio lighting` - 摄影棚灯光
- `cinematic lighting` - 电影光效
- `volumetric lighting` - 体积光
- `backlight` - 背光

**视角：**
- `close-up` - 特写
- `portrait` - 肖像
- `wide angle` - 广角
- `bird's eye view` - 鸟瞰
- `low angle` - 仰视
- `first person view` - 第一人称

---

### 3️⃣ 常用参数

**比例参数 `--ar`：**
```
--ar 16:9   # 横屏（适合视频封面）
--ar 9:16   # 竖屏（适合手机壁纸）
--ar 1:1    # 正方形（适合头像）
--ar 4:3    # 传统照片比例
```

**版本参数 `--v`：**
```
--v 6      # 最新版本（推荐）
--v 5.2    # 稳定版本
--v 4      # 老版本（某些风格更好）
```

**风格化 `--s`：**
```
--s 100    # 低风格化（更忠实提示词）
--s 250    # 中等（默认）
--s 750    # 高风格化（更艺术化）
```

**混乱度 `--c`：**
```
--c 0      # 低混乱（4张图很相似）
--c 50     # 中等（默认）
--c 100    # 高混乱（4张图差异大）
```

**权重 `::`：**
```
# 调整关键词权重
hot dog::2      # 狗的权重 ×2
hot::1.5 dog::1 # 热的权重 1.5，狗的权重 1
```

---

## 💡 实战案例

### 案例1：LOGO 设计

**提示词：**
```
/imagine prompt: minimal logo of a mountain, simple, clean, vector, black and white --no shading, gradient
```

**技巧：**
- 用 `minimal` 保持简洁
- 用 `--no` 排除不想要的元素
- 生成多次，挑选最好的

---

### 案例2：社交媒体封面

**提示词：**
```
/imagine prompt: abstract technology background, blue and purple gradient, geometric shapes, modern, futuristic --ar 16:9 --v 6
```

**技巧：**
- 用 `--ar 16:9` 设置横屏比例
- 用 `abstract` 避免具体物体
- 用渐变色增加视觉冲击

---

### 案例3：人物肖像

**提示词：**
```
/imagine prompt: portrait of a young woman, natural light, soft focus, dreamy atmosphere, film photography --ar 2:3 --v 6
```

**技巧：**
- 用 `portrait` 强调人物
- 用 `natural light` 避免生硬
- 用 `film photography` 增加质感

---

### 案例4：产品图

**提示词：**
```
/imagine prompt: luxury watch product photography, studio lighting, white background, professional, high detail --no watermark, text
```

**技巧：**
- 用 `product photography` 确保商业感
- 用 `studio lighting` 控制光线
- 用 `--no` 排除干扰元素

---

## 🔧 高级技巧

### 1️⃣ 图片垫图（Image Prompt）

**用法：**
```
/imagine prompt: [图片链接] 一只猫，赛博朋克风格
```

**效果：** AI 会参考你上传的图片风格

**步骤：**
1. 先上传一张图片到 Discord
2. 复制图片链接
3. 在 `/imagine` 中粘贴链接 + 描述

---

### 2️⃣ 融合图片（Blend）

**用法：**
```
/blend [图片1] [图片2]
```

**效果：** 两张图片融合成一张新图

**应用：**
- 人脸融合
- 风格迁移
- 创意合成

---

### 3️⃣ 局部重绘（Vary Region）

**用法：**
1. 放大一张图（U 按钮）
2. 点击 `Vary (Region)` 按钮
3. 选择要修改的区域
4. 输入新的描述

**应用：**
- 修改人物表情
- 替换背景
- 添加/删除物体

---

### 4️⃣ 平移扩展（Pan）

**用法：**
1. 生成一张图
2. 点击 `⬅️ ➡️ ⬆️ ⬇️` 按钮
3. AI 会在这个方向扩展画面

**应用：**
- 制作横幅图
- 扩展背景
- 创建全景图

---

### 5️⃣ 缩放（Zoom）

**用法：**
1. 生成一张图
2. 点击 `Zoom Out 2x` 或 `Zoom Out 1.5x`
3. AI 会在保持主体的同时缩小画面

**应用：**
- 调整构图
- 添加更多背景
- 修复裁剪问题

---

## 📊 MidJourney 版本对比

| 版本 | 特点 | 适用场景 |
|------|------|----------|
| **V6** | 最新，写实最强 | 通用场景 |
| **V5.2** | 稳定，风格化强 | 艺术创作 |
| **V4** | 老版本，某些风格更好 | 特殊需求 |
| **Niji 6** | 动漫专精 | 二次元创作 |

**切换版本：**
```
/settings
# 然后选择 MJ Version 6
```

---

## 💰 订阅计划详解

| 计划 | 价格 | 快速时长 | 放松时长 | 特点 |
|------|------|---------|---------|------|
| **Basic** | $10/月 | 3.3小时 | - | 入门级 |
| **Standard** | $30/月 | 15小时 | 无限 | 推荐 |
| **Pro** | $60/月 | 30小时 | 无限 | 隐私模式 |
| **Mega** | $120/月 | 60小时 | 无限 | 企业级 |

**快速 vs 放松：**
- **快速模式**：几十秒生成
- **放松模式**：几分钟到几十分钟（排队）

**建议：**
- 新手：Basic（先体验）
- 日常使用：Standard（性价比高）
- 商业用途：Pro（隐私保护）

---

## ❓ 常见问题

<details>
<summary><b>Q: MidJourney 免费吗？</b></summary>

**A: 目前不免费了。**

- 以前有免费试用（25张图）
- 现在必须订阅才能使用
- 最便宜的 Basic $10/月
</details>

<details>
<summary><b>Q: 生成的图片版权归谁？</b></summary>

**A: 付费用户拥有商用权。**

- 付费用户：可商用
- 生成的图片版权归你
- 但要注意肖像权（不能生成名人）
</details>

<details>
<summary><b>Q: 为什么我的图不如别人的好看？</b></summary>

**A: Prompt 技巧很重要。**

1. 多用风格关键词
2. 参考别人的优秀 Prompt
3. 用 `--s` 调整风格化
4. 多生成几次，挑最好的
</details>

<details>
<summary><b>Q: 可以生成中文描述吗？</b></summary>

**A: 不支持，必须用英文。**

- MidJourney 只理解英文
- 可以用翻译工具
- 或用 ChatGPT 帮你翻译
</details>

<details>
<summary><b>Q: 如何获得更多 Prompt 灵感？</b></summary>

**A: 参考 Midjourney Gallery。**

- 访问 [midjourney.com/showcase](https://midjourney.com/showcase)
- 查看优秀作品和 Prompt
- 在 Discord 中搜索关键词
</details>

---

## 🎯 学习路径

**第 1 天：基础使用**
- 注册和订阅
- 生成第一张图
- 理解 U/V 按钮

**第 1 周：掌握 Prompt**
- 学习风格关键词
- 尝试不同参数
- 模仿优秀作品

**第 1 月：熟练创作**
- 形成自己的风格
- 掌握高级技巧
- 应用于实际项目

**长期：商业应用**
- LOGO 设计
- 产品图
- 社交媒体素材

---

## 📚 推荐资源

**官方资源：**
- [MidJourney 官网](https://midjourney.com)
- [官方文档](https://docs.midjourney.com)
- [MidJourney Gallery](https://midjourney.com/showcase)

**学习网站：**
- [PromptHero](https://prompthero.com) - Prompt 搜索引擎
- [MidJourney Prompts](https://midjourney-prompt.com) - Prompt 教程

**社区：**
- MidJourney Discord 官方服务器
- Twitter/X: @midjourney

---

**💡 小贴士：** MidJourney 的核心是"多尝试"，同样的 Prompt 每次生成都不一样，多生成几次，总有一张让你满意！
