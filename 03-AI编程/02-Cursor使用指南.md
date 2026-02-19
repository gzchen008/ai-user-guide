# Cursor使用指南

> 让VS Code进化成AI编程神器

---

## 🤔 什么是Cursor？

**Cursor = VS Code + AI超能力**

简单来说，Cursor是一个基于VS Code的代码编辑器，集成了强大的AI功能，让编程效率提升10倍。

**Cursor的核心功能：**
- 🤖 **AI对话** - 在编辑器里直接问AI
- ⚡ **智能补全** - 写一行，AI帮你写剩下的
- 🔧 **代码重构** - 一键优化代码
- 📖 **代码解释** - 选中代码，AI解释给你听
- 🐛 **Bug修复** - AI帮你找问题

---

## 🆚 Cursor vs VS Code

| 功能 | VS Code | Cursor |
|------|---------|--------|
| 基础编辑 | ✅ | ✅ |
| 插件生态 | ✅ 丰富 | ✅ 兼容VS Code插件 |
| AI对话 | ❌ | ✅ 内置 |
| 智能补全 | ⚠️ 基础 | ✅ 超强 |
| 代码解释 | ❌ | ✅ |
| Bug修复 | ❌ | ✅ |
| 价格 | 免费 | 免费版 + Pro $20/月 |

**总结：** Cursor = VS Code的所有功能 + 强大的AI

---

## 🚀 快速开始

### 1️⃣ 下载安装

**官网：** [cursor.sh](https://cursor.sh)

**支持系统：**
- ✅ macOS
- ✅ Windows
- ✅ Linux

**安装步骤：**
1. 访问官网下载
2. 双击安装
3. 登录账号（支持GitHub登录）
4. 开始使用

---

### 2️⃣ 基础配置

**推荐设置：**

```json
// 打开设置（Cmd+,）- 搜索"cursor"
{
  // AI模型选择
  "cursor.aiModel": "gpt-4",
  
  // 自动补全
  "cursor.autocomplete": true,
  
  // 快捷键
  "cursor.chatKeybinding": "cmd+k",
  
  // 字体大小
  "editor.fontSize": 14,
  
  // 主题
  "workbench.colorTheme": "One Dark Pro"
}
```

---

### 3️⃣ 导入VS Code配置

**一键迁移：**
1. 打开Cursor
2. 命令面板（Cmd+Shift+P）
3. 输入 "Import"
4. 选择 "Import VS Code Settings"
5. 完成！

**会迁移：**
- ✅ 插件
- ✅ 快捷键
- ✅ 主题
- ✅ 设置

---

## 🎯 核心功能详解

### 1️⃣ AI对话（Chat）

**快捷键：** `Cmd+L`（打开聊天面板）

**使用场景：**

```
你：帮我写一个Python爬虫，抓取新闻网站标题

AI：生成完整代码

你：加上异常处理和日志记录

AI：优化代码

你：保存为 spider.py

AI：帮你创建文件
```

**特点：**
- ✅ 理解整个项目上下文
- ✅ 可以直接修改文件
- ✅ 支持多轮对话
- ✅ 可以引用代码块

---

### 2️⃣ 智能补全（Tab）

**快捷键：** `Tab`（接受建议）

**使用方式：**
1. 写一行代码
2. AI预测你的下一步
3. 按Tab接受建议
4. 继续写，AI继续补全

**示例：**

```python
# 你写：
def calculate_average(numbers):

# AI补全：
    if len(numbers) == 0:
        return 0
    return sum(numbers) / len(numbers)
```

**特点：**
- ✅ 基于上下文补全
- ✅ 学习你的编码风格
- ✅ 支持多语言
- ✅ 速度极快

---

### 3️⃣ 内联编辑（Inline Edit）

**快捷键：** `Cmd+K`

**使用方式：**
1. 选中代码
2. 按 `Cmd+K`
3. 输入修改需求
4. AI直接修改代码

**示例：**

```python
# 选中这段代码
def sort_list(data):
    return data.sort()  # ❌ 有bug

# 按 Cmd+K，输入：修复这个bug
def sort_list(data):
    return sorted(data)  # ✅ AI修复
```

---

### 4️⃣ 代码解释（Explain）

**快捷键：** 选中代码 → 右键 → "Explain Code"

**使用场景：**

```python
# 看不懂这段代码？
async def fetch_data(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()

# 选中代码，按Explain，AI解释：
这段代码使用了 aiohttp 库进行异步HTTP请求：
1. 创建异步会话
2. 发送GET请求
3. 等待响应
4. 返回JSON数据
```

**适合：**
- ✅ 学习新代码库
- ✅ Code Review
- ✅ 理解复杂逻辑

---

### 5️⃣ Bug修复（Fix Bug）

**快捷键：** 选中代码 → 右键 → "Fix Bug"

**使用方式：**
1. 选中报错的代码
2. 右键选择 "Fix Bug"
3. AI分析问题
4. 提供修复方案

**示例：**

```python
# 报错代码
data = []
print(data[0])  # IndexError

# AI修复：
if len(data) > 0:
    print(data[0])
else:
    print("列表为空")
```

---

## 💡 高级技巧

### 1️⃣ @符号引用

**在对话中引用文件或代码：**

```
@main.py 帮我优化这个文件的性能
@utils.py 这个函数有bug吗？
@README.md 帮我更新文档
```

**支持的引用：**
- `@文件名` - 引用整个文件
- `@文件夹` - 引用整个目录
- `@代码块` - 引用选中的代码
- `@终端` - 引用终端输出

---

### 2️⃣ .cursorrules 文件

**创建项目规则文件：**

```bash
# 在项目根目录创建 .cursorrules
```

**示例内容：**

```
这是一个Python项目
使用Python 3.11
遵循PEP 8规范
使用pytest测试
优先使用类型注解
```

**作用：** AI会遵循这些规则生成代码

---

### 3️⃣ 多文件编辑

**一次性修改多个文件：**

```
帮我重构这个项目：
1. 把所有 print() 改成 logging
2. 添加类型注解
3. 创建 requirements.txt
```

**Cursor会：**
- ✅ 分析整个项目
- ✅ 批量修改文件
- ✅ 确保一致性

---

### 4️⃣ Git集成

**AI辅助Git操作：**

```
帮我写一个commit message
帮我生成PR描述
帮我review这个commit
```

---

## 📊 使用场景

### 场景1：快速原型开发

**需求：** 做一个API接口

**Cursor流程：**
```
1. Cmd+L 打开对话
2. 描述需求："创建一个FastAPI接口，返回用户列表"
3. AI生成代码
4. 按Tab补全细节
5. Cmd+K 优化代码
6. 测试运行
```

**时间：** 传统2小时 → Cursor 10分钟

---

### 场景2：代码重构

**需求：** 优化遗留代码

**Cursor流程：**
```
1. 选中老旧代码
2. Cmd+K："重构为现代Python风格，添加类型注解"
3. AI重构
4. 运行测试
5. Cmd+K："添加单元测试"
```

**效果：** 代码质量提升，可维护性增强

---

### 场景3：学习新技术

**需求：** 学习React

**Cursor流程：**
```
1. 选中React代码
2. 右键 → Explain Code
3. AI解释每个部分
4. 修改代码实验
5. 看AI如何优化
```

**效果：** 边做边学，效率最高

---

## 🎯 最佳实践

### ✅ DO（推荐做法）

1. **善用@引用**
   ```
   @main.py 这个函数的用途是什么？
   ```

2. **创建.cursorrules**
   ```
   告诉AI你的项目规范
   ```

3. **多轮对话优化**
   ```
   第一次生成的代码不满意？
   继续对话："优化性能"、"添加异常处理"
   ```

4. **结合Git使用**
   ```
   每次AI修改后，review diff再commit
   ```

### ❌ DON'T（避免做法）

1. **完全信任AI**
   ```
   ❌ AI生成的代码直接上生产
   
   ✅ 测试验证后再使用
   ```

2. **不学习基础**
   ```
   ❌ 连Git都不会就做复杂项目
   
   ✅ 基础工具还是要掌握
   ```

3. **过度依赖**
   ```
   ❌ 所有代码都让AI写
   
   ✅ 复杂逻辑自己设计，AI帮你实现
   ```

---

## 💰 价格对比

| 版本 | 价格 | 功能 |
|------|------|------|
| Free | 免费 | • 基础AI功能<br>• 2000次补全/月<br>• GPT-4额度有限 |
| Pro | $20/月 | • 无限补全<br>• GPT-4无限制<br>• Claude 3.5<br>• 优先支持 |

**建议：**
- 新手：先用免费版
- 日常开发：Pro版性价比高

---

## ❓ 常见问题

<details>
<summary><b>Q: Cursor能完全替代VS Code吗？</b></summary>

**A: 完全可以！**

- Cursor兼容所有VS Code插件
- 界面和快捷键完全一样
- 额外增加了AI功能
- 可以无缝迁移
</details>

<details>
<summary><b>Q: AI补全准确吗？</b></summary>

**A: 大部分场景很准确。**

- 常规代码：准确率90%+
- 复杂逻辑：可能需要调整
- 建议：接受补全后检查一下
</details>

<details>
<summary><b>Q: 支持哪些编程语言？</b></summary>

**A: 几乎所有主流语言。**

- ✅ Python, JavaScript, TypeScript
- ✅ Java, C++, Go, Rust
- ✅ HTML, CSS, SQL
- ✅ 配置文件（JSON, YAML）
</details>

<details>
<summary><b>Q: 代码会被上传到AI服务器吗？</b></summary>

**A: 会，但有隐私模式。**

- 默认：代码片段会发送到AI
- 隐私模式：不保存你的代码
- 企业版：本地部署
</details>

---

## 🎯 下一步

学完Cursor基础，接下来：

👉 [Claude Code技巧](./03-ClaudeCode技巧.md) - 更强大的AI编程助手

👉 [MCP开发](./05-MCP开发.md) - 扩展AI能力

---

**💡 小贴士：** Cursor最大的价值是"让AI理解你的项目"，多用@引用，让AI更懂你的代码！
