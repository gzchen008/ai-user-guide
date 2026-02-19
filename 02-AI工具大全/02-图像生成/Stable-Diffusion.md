# Stable Diffusion 完全指南

> 免费开源的 AI 绘图神器

---

## 🤔 什么是 Stable Diffusion？

**Stable Diffusion = 开源免费的 AI 绘图工具**

简单来说，Stable Diffusion 是一个开源的 AI 图像生成模型，你可以免费使用，甚至可以在本地电脑上运行。

**Stable Diffusion 的核心优势：**
- 💰 **完全免费** - 无需订阅
- 🔒 **隐私保护** - 本地运行，数据不上传
- 🎨 **可定制** - 无数模型和插件
- 💻 **离线使用** - 不依赖网络

**一句话总结：** Stable Diffusion 是免费且强大的 AI 绘图解决方案

---

## 🆚 Stable Diffusion vs 其他工具

| 工具 | 价格 | 可定制性 | 隐私 | 硬件要求 |
|------|------|---------|------|---------|
| **Stable Diffusion** | 免费 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 中高 |
| MidJourney | $10/月起 | ⭐⭐ | ⭐⭐⭐ | 无 |
| DALL-E 3 | $20/月 | ⭐⭐ | ⭐⭐⭐ | 无 |
| Ideogram | 免费/付费 | ⭐⭐⭐ | ⭐⭐⭐ | 无 |

**选择建议：**
- 有显卡、追求免费 → **Stable Diffusion**
- 没显卡、追求易用 → MidJourney / DALL-E 3
- 追求商业质量 → MidJourney

---

## 🚀 快速开始

### 方式1：本地安装（推荐）

**硬件要求：**
- GPU: NVIDIA RTX 3060 或以上（8GB+ 显存）
- 内存: 16GB+
- 硬盘: 20GB+ 可用空间

**安装步骤：**

#### 1️⃣ 安装 Python

```bash
# 下载 Python 3.10.9
https://www.python.org/downloads/release/python-3109/

# 安装时勾选 "Add Python to PATH"
```

#### 2️⃣ 安装 Git

```bash
# 下载 Git
https://git-scm.com/downloads

# 安装完成后，打开命令行验证
git --version
```

#### 3️⃣ 安装 Stable Diffusion WebUI

```bash
# 克隆项目
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git

# 进入目录
cd stable-diffusion-webui

# 运行（首次会自动下载模型，约 4GB）
./webui.sh  # macOS/Linux
webui.bat   # Windows
```

#### 4️⃣ 访问 WebUI

```
打开浏览器，访问：http://127.0.0.1:7860
```

---

### 方式2：在线平台（无需显卡）

**推荐平台：**

| 平台 | 网址 | 特点 |
|------|------|------|
| **Civitai** | [civitai.com](https://civitai.com) | 模型社区 + 在线生成 |
| **Tensor.art** | [tensor.art](https://tensor.art) | 免费生成额度 |
| **RunPod** | [runpod.io](https://runpod.io) | 云端 GPU 租赁 |
| **Google Colab** | [colab.research.google.com](https://colab.research.google.com) | 免费 GPU（限时） |

---

## 🎨 核心概念

### 1️⃣ 模型（Model）

**模型 = AI 的"画风"**

不同模型生成不同风格：
- **SD 1.5** - 通用模型，速度快
- **SDXL** - 高质量，需要更多显存
- **Realistic Vision** - 写实风格
- **Anything V5** - 动漫风格
- **DreamShaper** - 通用高质量

**下载模型：**
- [Civitai](https://civitai.com/models) - 最大模型社区
- [Hugging Face](https://huggingface.co/models)

**安装模型：**
```
将模型文件（.safetensors）放到：
stable-diffusion-webui/models/Stable-diffusion/
```

---

### 2️⃣ 提示词（Prompt）

**正向提示词（Positive）：**
```
a beautiful girl, long hair, white dress, sunset, cinematic lighting, 8k, highly detailed
```

**负向提示词（Negative）：**
```
ugly, deformed, blurry, low quality, watermark, text
```

---

### 3️⃣ 采样方法（Sampler）

**常用采样器：**

| 采样器 | 速度 | 质量 | 推荐场景 |
|--------|------|------|----------|
| **DPM++ 2M Karras** | 快 | 高 | 通用（推荐） |
| **Euler a** | 最快 | 中 | 快速预览 |
| **DDIM** | 中 | 高 | 细节控制 |
| **UniPC** | 快 | 高 | 新兴推荐 |

---

### 4️⃣ 采样步数（Steps）

**推荐值：**
- **20-30 步** - 通用场景
- **30-50 步** - 追求细节
- **10-20 步** - 快速测试

**注意：** 步数越高不一定越好，可能过拟合

---

### 5️⃣ CFG Scale（提示词相关性）

**推荐值：**
- **7** - 默认值
- **5-8** - 通用范围
- **9-12** - 严格遵循提示词
- **3-5** - 更自由发挥

---

## 💡 实战案例

### 案例1：写实人像

**设置：**
- 模型：Realistic Vision V5
- 采样器：DPM++ 2M Karras
- 步数：30
- CFG：7
- 尺寸：512x768

**正向提示词：**
```
portrait of a young woman, natural beauty, soft lighting, photorealistic, 8k, high detail, professional photography, bokeh background
```

**负向提示词：**
```
ugly, deformed, bad anatomy, extra fingers, cartoon, anime, 3d render
```

---

### 案例2：动漫角色

**设置：**
- 模型：Anything V5
- 采样器：DPM++ 2M Karras
- 步数：25
- CFG：7
- 尺寸：512x768

**正向提示词：**
```
anime girl, cute face, long hair, school uniform, cherry blossoms, spring, soft lighting, high quality, detailed
```

**负向提示词：**
```
ugly, deformed, realistic, photo, 3d, bad anatomy
```

---

### 案例3：风景照片

**设置：**
- 模型：DreamShaper
- 采样器：DPM++ 2M Karras
- 步数：30
- CFG：7
- 尺寸：768x512

**正向提示词：**
```
beautiful landscape, mountains, lake, sunset, golden hour, dramatic sky, photorealistic, 8k, cinematic, wide angle
```

**负向提示词：**
```
blurry, low quality, cartoon, anime, text, watermark
```

---

## 🔧 高级技巧

### 1️⃣ LoRA（风格插件）

**LoRA = 轻量级风格模型**

**用法：**
1. 下载 LoRA 文件
2. 放到 `models/Lora/` 目录
3. 在提示词中引用：
   ```
   <lora:realistic:0.7> beautiful woman
   ```

**常用 LoRA：**
- **Realistic** - 写真人像
- **Anime Lineart** - 动漫线稿
- **Studio Lighting** - 摄影棚光效
- **Vintage Photo** - 复古照片

---

### 2️⃣ ControlNet（精准控制）

**ControlNet = 用参考图控制生成**

**常用控制类型：**
- **Canny** - 边缘检测
- **Pose** - 姿势控制
- **Depth** - 深度图
- **OpenPose** - 人体姿态

**使用步骤：**
1. 安装 ControlNet 插件
2. 上传参考图
3. 选择控制类型
4. 调整权重
5. 生成

---

### 3️⃣ 图生图（Img2Img）

**用法：**
1. 切换到 "img2img" 标签页
2. 上传参考图
3. 调整 "Denoising strength"（重绘幅度）
   - **0.3-0.5** - 轻微修改
   - **0.5-0.7** - 中等修改
   - **0.7-0.9** - 大幅修改
4. 输入提示词
5. 生成

---

### 4️⃣ 局部重绘（Inpaint）

**用法：**
1. 在 img2img 页面选择 "Inpaint"
2. 上传图片
3. 用画笔涂抹要修改的区域
4. 输入描述
5. 调整 "Denoising strength"（0.7-1.0）
6. 生成

**应用：**
- 修改人物表情
- 替换服装
- 删除不需要的物体
- 修复缺陷

---

### 5️⃣ 高清修复（Hires. Fix）

**设置：**
- **Upscaler**: R-ESRGAN 4x+ 或 4x-UltraSharp
- **Upscale by**: 1.5-2.0
- **Denoising strength**: 0.3-0.5

**效果：**
- 分辨率提升 1.5-2 倍
- 细节更丰富
- 适合打印或大屏展示

---

## 📊 常用模型推荐

### 写实类

| 模型 | 特点 | 适用场景 |
|------|------|----------|
| **Realistic Vision V5** | 最写实 | 人像、产品 |
| **Deliberate V3** | 通用写实 | 风景、建筑 |
| **Photon** | 高细节 | 商业用途 |
| **MajicMix** | 艺术写实 | 创意摄影 |

---

### 动漫类

| 模型 | 特点 | 适用场景 |
|------|------|----------|
| **Anything V5** | 通用动漫 | 二次元角色 |
| **DreamShaper Anime** | 高质量 | 动漫插画 |
| **Pastel Mix** | 柔和风格 | 少女系 |
| **Counterfeit V3** | 日系动漫 | 动漫角色 |

---

### 艺术类

| 模型 | 特点 | 适用场景 |
|------|------|----------|
| **DreamShaper** | 通用艺术 | 插画、概念图 |
| **OpenJourney** | MidJourney 风格 | 艺术创作 |
| **RF Inpainting** | 图像修复 | 修图 |
| **Colorful** | 色彩丰富 | 概念艺术 |

---

## 🎯 学习路径

**第 1 天：基础使用**
- 安装 Stable Diffusion
- 生成第一张图
- 理解提示词

**第 1 周：模型和参数**
- 尝试不同模型
- 掌握采样器和步数
- 学习 LoRA

**第 1 月：高级技巧**
- 掌握 ControlNet
- 熟练使用 Inpaint
- 形成工作流

**长期：专业应用**
- 模型训练
- 插件开发
- 商业应用

---

## ❓ 常见问题

<details>
<summary><b>Q: 没有显卡能用 Stable Diffusion 吗？</b></summary>

**A: 可以，用在线平台。**

- Civitai、Tensor.art 等平台提供免费额度
- RunPod、Lambda Labs 等提供云 GPU 租赁
- Google Colab 提供免费 GPU（限时）
</details>

<details>
<summary><b>Q: 生成速度很慢怎么办？</b></summary>

**A: 优化方法：**

1. 降低分辨率（512x512）
2. 减少步数（20步）
3. 使用 xFormers 加速
4. 升级显卡
</details>

<details>
<summary><b>Q: 生成的图质量不好？</b></summary>

**A: 可能的原因：**

1. 模型不适合场景
2. 提示词不精准
3. 参数设置不当
4. 步数太少

**解决：** 换模型、优化提示词、参考优秀作品
</details>

<details>
<summary><b>Q: 如何找到合适的模型？</b></summary>

**A: 访问 Civitai。**

1. 搜索关键词（如 "realistic"）
2. 按下载量排序
3. 查看示例图
4. 阅读评论
</details>

<details>
<summary><b>Q: 可以商用吗？</b></summary>

**A: 取决于模型许可证。**

- SD 1.5 / SDXL: 可商用
- 社区模型: 查看许可证
- 建议: 阅读模型说明
</details>

---

## 📚 推荐资源

**模型下载：**
- [Civitai](https://civitai.com) - 最大模型社区
- [Hugging Face](https://huggingface.co/models) - 官方模型库

**学习网站：**
- [Stable Diffusion Art](https://stable-diffusion-art.com)
- [Reddit r/StableDiffusion](https://reddit.com/r/StableDiffusion)

**工具：**
- [AUTOMATIC1111 WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) - 节点式工作流

---

**💡 小贴士：** Stable Diffusion 的强大在于"可定制"，多尝试不同的模型和参数组合，找到最适合你的风格！
