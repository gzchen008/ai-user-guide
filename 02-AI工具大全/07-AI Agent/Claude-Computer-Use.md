# Claude Computer Use 指南

> Anthropic 出品的电脑操作 AI Agent

---

## 📖 简介

**Computer Use** 是 Anthropic 在 Claude 中推出的 AI Agent 能力，让 Claude 能够看到屏幕、移动鼠标、点击按钮、输入文字——像人类一样操作电脑。

**开发商：** Anthropic
**发布时间：** 2024年10月（Beta）
**定位：** 桌面级 AI Agent
**支持模型：** Claude Sonnet 4、Claude Opus 4

---

## ✨ 核心能力

### 1. 屏幕理解
- 截屏分析当前界面
- 识别按钮、菜单、输入框
- 理解页面布局和内容

### 2. 鼠标键盘操作
- 移动光标到指定位置
- 点击、双击、右键
- 键盘输入文字和快捷键
- 滚动页面

### 3. 多应用协作
- 在不同应用之间切换
- 跨应用复制粘贴
- 同时操作多个窗口

---

## 🎯 适合场景

| 场景 | 示例 |
|------|------|
| 数据录入 | 从网页提取信息填入表格 |
| 自动化测试 | 自动操作软件进行测试 |
| 批量操作 | 批量重命名文件、整理文件夹 |
| 开发辅助 | 在 IDE 中执行复杂操作 |
| 研究分析 | 打开多个网页收集信息 |

---

## 🛠️ 使用方式

### 方式1：Claude.ai（网页版）

在 Claude 对话中直接使用：
1. 授权 Claude 访问你的屏幕
2. 描述你想让它做什么
3. Claude 会截屏→分析→执行→再截屏，循环操作

### 方式2：API 调用（开发者）

```python
import anthropic

client = anthropic.Anthropic()

# 发送 Computer Use 请求
response = client.beta.messages.create(
    model="claude-sonnet-4-20250514",
    tools=[
        {
            "type": "computer_20250124",
            "name": "computer",
            "display_width_px": 1920,
            "display_height_px": 1080,
        }
    ],
    messages=[
        {"role": "user", "content": "帮我打开浏览器搜索今天的天气"}
    ],
    betas=["computer-use-2025-01-24"],
)
```

### 方式3：Claude Code（终端）

Claude Code 中也可调用 Computer Use 能力进行更复杂的自动化。

---

## 💡 实战案例

### 案例1：自动填写报销单
```
你：帮我把桌面上的发票信息填到报销系统里

Claude 执行过程：
1. 打开发票图片，读取金额和日期
2. 打开公司报销系统
3. 填写报销项目
4. 上传发票图片
5. 提交报销

耗时：2分钟（人工需要10分钟）
```

### 案例2：跨应用数据整理
```
你：把邮件里的客户信息整理到 Excel 表格里

Claude 执行过程：
1. 打开邮件客户端
2. 读取客户邮件信息
3. 打开 Excel
4. 按格式填入数据
5. 保存文件

耗时：5分钟（人工需要30分钟）
```

---

## 🆚 对比其他 Agent

| 维度 | Claude Computer Use | Manus | OpenAI Operator |
|------|-------------------|-------|----------------|
| 操作对象 | 整台电脑 | 主要网页 | 主要网页 |
| 技术门槛 | 中（需要 API 知识） | 低 | 低 |
| 灵活性 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| 安全控制 | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| 适合人群 | 开发者/技术用户 | 普通用户 | 普通用户 |

**选择建议：**
- 想操控整台电脑（任意应用）→ Claude Computer Use
- 想简单操作网页 → Manus 或 Operator
- 开发者需要 API → Claude Computer Use

---

## ⚠️ 安全注意事项

1. **操作可见**：所有操作实时显示在屏幕上，可随时中断
2. **敏感操作**：涉及密码/支付时建议手动处理
3. **沙盒模式**：开发测试时建议使用虚拟机或沙盒环境
4. **权限最小化**：只授予必要的权限

---

## 📊 定价

| 使用方式 | 费用 |
|---------|------|
| Claude.ai（Pro） | $20/月起 |
| API 调用 | 按 token 计费 |

> ⚠️ Computer Use 目前仍为 Beta 功能，可能存在不稳定性。

---

## 🔮 未来展望

- 更精准的屏幕理解和操作
- 支持更复杂的多步骤任务
- 更好的错误恢复机制
- 与 Claude 生态深度整合

---

## 🔗 相关链接

- 官方文档：https://docs.anthropic.com/en/docs/agents-and-tools/computer-use
- [Manus 指南](./Manus.md)
- [OpenAI Operator 指南](./OpenAI-Operator.md)
- [AI Agent 总览](./README.md)

---

**💡 小贴士：** Computer Use 是目前最灵活的桌面级 AI Agent，适合需要操作多种桌面应用的场景。如果你是开发者，强烈建议通过 API 体验！
