# 09-AI Agent 开发

> 打造能自主思考、执行任务的 AI 助手

---

## 📖 项目介绍

AI Agent（智能代理）是能够自主感知、决策、执行任务的 AI 系统。本教程带你从零开发一个实用的 AI Agent。

**难度：** ⭐⭐⭐⭐⭐（高级）

**你将学到：**
- Agent 核心架构
- 工具调用（Function Calling）
- 记忆系统设计
- 多 Agent 协作

---

## 🎯 项目目标

我们要开发一个 **「智能任务助手」** Agent：

**核心能力：**
- 理解用户自然语言指令
- 调用各种工具（搜索、计算、文件操作等）
- 记住对话历史和用户偏好
- 自动分解复杂任务

**技术栈：**
- 语言模型：DeepSeek / OpenAI API
- 工具调用：Function Calling
- 记忆存储：本地 JSON 文件
- 界面：命令行 / Web UI

---

## 🧠 Agent 架构

### 核心组件

```
┌─────────────────────────────────────────────────┐
│                   AI Agent                      │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌─────────┐  ┌─────────┐  ┌─────────────────┐ │
│  │ 感知层   │→│ 决策层   │→│    执行层       │ │
│  │         │  │         │  │                 │ │
│  │ 用户输入 │  │ LLM推理  │  │ 工具调用       │ │
│  │ 环境信息 │  │ 任务分解 │  │ 动作执行       │ │
│  └─────────┘  └─────────┘  └─────────────────┘ │
│                      ↓                          │
│              ┌──────────────┐                   │
│              │   记忆层      │                   │
│              │              │                   │
│              │ 短期记忆     │                   │
│              │ 长期记忆     │                   │
│              │ 用户画像     │                   │
│              └──────────────┘                   │
└─────────────────────────────────────────────────┘
```

### 工作流程

```
1. 用户输入 → 2. 感知理解 → 3. 检索记忆
     ↓
4. 推理决策 → 5. 调用工具 → 6. 执行动作
     ↓
7. 生成响应 → 8. 更新记忆 → 9. 返回用户
```

---

## 📁 项目结构

```
ai-agent/
├── src/
│   ├── index.js            # 入口文件
│   ├── agent.js            # Agent 核心类
│   ├── llm.js              # LLM 调用封装
│   ├── memory.js           # 记忆系统
│   ├── tools/              # 工具集合
│   │   ├── index.js
│   │   ├── search.js       # 搜索工具
│   │   ├── calculator.js   # 计算器
│   │   ├── weather.js      # 天气查询
│   │   └── file.js         # 文件操作
│   └── utils/              # 工具函数
│       ├── logger.js
│       └── parser.js
├── data/
│   ├── memory.json         # 记忆存储
│   └── user_profile.json   # 用户画像
├── .env                    # 环境变量
├── package.json
└── README.md
```

---

## 🚀 开发步骤

### 第 1 步：初始化项目

**向 AI 提问：**
```
帮我初始化一个 AI Agent 项目，要求：
- 使用 Node.js + ES Modules
- 安装 openai、axios、chalk 等依赖
- 配置环境变量管理
- 创建基本目录结构
```

**package.json：**
```json
{
  "name": "ai-agent",
  "version": "1.0.0",
  "type": "module",
  "description": "智能任务助手 Agent",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js"
  },
  "dependencies": {
    "openai": "^4.20.0",
    "axios": "^1.6.0",
    "chalk": "^5.3.0",
    "dotenv": "^16.3.0",
    "inquirer": "^9.2.0"
  }
}
```

**.env：**
```env
# 使用 DeepSeek API（便宜好用）
DEEPSEEK_API_KEY=your_deepseek_api_key
DEEPSEEK_BASE_URL=https://api.deepseek.com

# 或使用 OpenAI
# OPENAI_API_KEY=your_openai_api_key
```

---

### 第 2 步：LLM 调用封装

**src/llm.js：**

```javascript
// src/llm.js - LLM 调用封装

import OpenAI from 'openai';
import dotenv from 'dotenv';

dotenv.config();

// 支持多个 LLM 提供商
const createLLM = (provider = 'deepseek') => {
  const configs = {
    deepseek: {
      apiKey: process.env.DEEPSEEK_API_KEY,
      baseURL: process.env.DEEPSEEK_BASE_URL || 'https://api.deepseek.com',
      model: 'deepseek-chat'
    },
    openai: {
      apiKey: process.env.OPENAI_API_KEY,
      baseURL: 'https://api.openai.com/v1',
      model: 'gpt-4o-mini'
    }
  };

  const config = configs[provider];
  
  return {
    client: new OpenAI({
      apiKey: config.apiKey,
      baseURL: config.baseURL
    }),
    model: config.model
  };
};

// 聊天完成
export async function chatCompletion(messages, tools = [], options = {}) {
  const { client, model } = createLLM('deepseek');
  
  const response = await client.chat.completions.create({
    model: options.model || model,
    messages,
    tools: tools.length > 0 ? tools : undefined,
    tool_choice: tools.length > 0 ? 'auto' : undefined,
    temperature: options.temperature || 0.7,
    max_tokens: options.maxTokens || 2000
  });
  
  return response.choices[0];
}

// 流式输出
export async function* streamChat(messages, options = {}) {
  const { client, model } = createLLM('deepseek');
  
  const stream = await client.chat.completions.create({
    model: options.model || model,
    messages,
    temperature: options.temperature || 0.7,
    stream: true
  });
  
  for await (const chunk of stream) {
    const content = chunk.choices[0]?.delta?.content;
    if (content) {
      yield content;
    }
  }
}

export default { chatCompletion, streamChat };
```

---

### 第 3 步：工具系统

**src/tools/index.js：**

```javascript
// src/tools/index.js - 工具注册中心

import { searchTool, executeSearch } from './search.js';
import { calculatorTool, executeCalculate } from './calculator.js';
import { weatherTool, executeWeather } from './weather.js';
import { fileTool, executeFile } from './file.js';

// 工具定义（给 LLM 看的）
export const tools = [
  searchTool,
  calculatorTool,
  weatherTool,
  fileTool
];

// 工具执行映射
const toolExecutors = {
  search: executeSearch,
  calculate: executeCalculate,
  get_weather: executeWeather,
  file_operation: executeFile
};

// 执行工具
export async function executeTool(toolName, args) {
  const executor = toolExecutors[toolName];
  
  if (!executor) {
    return { error: `Unknown tool: ${toolName}` };
  }
  
  try {
    console.log(`🔧 执行工具: ${toolName}`);
    console.log(`📝 参数:`, args);
    
    const result = await executor(args);
    
    console.log(`✅ 结果:`, result);
    return result;
    
  } catch (error) {
    console.error(`❌ 工具执行失败:`, error.message);
    return { error: error.message };
  }
}
```

**src/tools/search.js：**

```javascript
// src/tools/search.js - 搜索工具

// 工具定义（Function Calling 格式）
export const searchTool = {
  type: 'function',
  function: {
    name: 'search',
    description: '搜索互联网获取信息。当用户询问时事、新闻、具体信息时使用。',
    parameters: {
      type: 'object',
      properties: {
        query: {
          type: 'string',
          description: '搜索关键词'
        },
        num_results: {
          type: 'integer',
          description: '返回结果数量，默认 3',
          default: 3
        }
      },
      required: ['query']
    }
  }
};

// 工具执行
export async function executeSearch(args) {
  const { query, num_results = 3 } = args;
  
  // 这里使用 DuckDuckGo Instant Answer API（免费）
  // 实际项目可以使用 Serper、SerpAPI 等
  
  const url = `https://api.duckduckgo.com/?q=${encodeURIComponent(query)}&format=json&no_html=1`;
  
  try {
    const response = await fetch(url);
    const data = await response.json();
    
    if (data.AbstractText) {
      return {
        success: true,
        answer: data.AbstractText,
        source: data.AbstractURL
      };
    }
    
    if (data.RelatedTopics && data.RelatedTopics.length > 0) {
      const results = data.RelatedTopics
        .slice(0, num_results)
        .filter(t => t.Text)
        .map(t => ({
          title: t.Text.split(' - ')[0],
          snippet: t.Text,
          url: t.FirstURL
        }));
      
      return {
        success: true,
        results
      };
    }
    
    return {
      success: false,
      message: '未找到相关信息'
    };
    
  } catch (error) {
    return {
      success: false,
      error: error.message
    };
  }
}
```

**src/tools/calculator.js：**

```javascript
// src/tools/calculator.js - 计算器工具

export const calculatorTool = {
  type: 'function',
  function: {
    name: 'calculate',
    description: '执行数学计算。支持加减乘除、幂运算、开方等。',
    parameters: {
      type: 'object',
      properties: {
        expression: {
          type: 'string',
          description: '数学表达式，如 "2 + 3 * 4"、"sqrt(16)"、"2^10"'
        }
      },
      required: ['expression']
    }
  }
};

export async function executeCalculate(args) {
  const { expression } = args;
  
  try {
    // 安全的数学表达式求值
    const sanitized = expression
      .replace(/[^0-9+\-*/().^%sqrt\s]/g, '')
      .replace(/\^/g, '**')
      .replace(/sqrt/g, 'Math.sqrt');
    
    // 使用 Function 构造器安全执行
    const result = new Function(`return ${sanitized}`)();
    
    return {
      success: true,
      expression,
      result
    };
    
  } catch (error) {
    return {
      success: false,
      error: '计算表达式无效',
      expression
    };
  }
}
```

**src/tools/weather.js：**

```javascript
// src/tools/weather.js - 天气工具

export const weatherTool = {
  type: 'function',
  function: {
    name: 'get_weather',
    description: '获取指定城市的天气信息。',
    parameters: {
      type: 'object',
      properties: {
        city: {
          type: 'string',
          description: '城市名称，如"北京"、"上海"、"广州"'
        }
      },
      required: ['city']
    }
  }
};

export async function executeWeather(args) {
  const { city } = args;
  
  // 使用 wttr.in（免费天气 API）
  const url = `https://wttr.in/${encodeURIComponent(city)}?format=j1`;
  
  try {
    const response = await fetch(url);
    const data = await response.json();
    
    const current = data.current_condition[0];
    
    return {
      success: true,
      city,
      temperature: `${current.temp_C}°C`,
      feels_like: `${current.FeelsLikeC}°C`,
      description: current.weatherDesc[0].value,
      humidity: `${current.humidity}%`,
      wind: `${current.windspeedKmph} km/h`,
      forecast: data.weather.slice(0, 3).map(day => ({
        date: day.date,
        max: `${day.maxtempC}°C`,
        min: `${day.mintempC}°C`
      }))
    };
    
  } catch (error) {
    return {
      success: false,
      error: `无法获取 ${city} 的天气信息`
    };
  }
}
```

**src/tools/file.js：**

```javascript
// src/tools/file.js - 文件操作工具

import fs from 'fs/promises';
import path from 'path';

export const fileTool = {
  type: 'function',
  function: {
    name: 'file_operation',
    description: '执行文件操作：读取、写入、列表等。',
    parameters: {
      type: 'object',
      properties: {
        action: {
          type: 'string',
          enum: ['read', 'write', 'list', 'delete'],
          description: '操作类型'
        },
        filepath: {
          type: 'string',
          description: '文件路径（相对路径）'
        },
        content: {
          type: 'string',
          description: '写入内容（仅 write 操作需要）'
        }
      },
      required: ['action', 'filepath']
    }
  }
};

const WORK_DIR = './data/workspace';

export async function executeFile(args) {
  const { action, filepath, content } = args;
  
  // 安全检查：只允许操作工作目录
  const fullPath = path.join(WORK_DIR, filepath);
  const normalized = path.normalize(fullPath);
  
  if (!normalized.startsWith(path.resolve(WORK_DIR))) {
    return { error: '不允许访问工作目录之外的文件' };
  }
  
  switch (action) {
    case 'read':
      try {
        const data = await fs.readFile(normalized, 'utf-8');
        return { success: true, content: data };
      } catch (error) {
        return { error: '文件不存在或无法读取' };
      }
      
    case 'write':
      try {
        await fs.mkdir(path.dirname(normalized), { recursive: true });
        await fs.writeFile(normalized, content, 'utf-8');
        return { success: true, message: '文件写入成功' };
      } catch (error) {
        return { error: '文件写入失败' };
      }
      
    case 'list':
      try {
        const files = await fs.readdir(normalized);
        return { success: true, files };
      } catch (error) {
        return { error: '目录不存在' };
      }
      
    case 'delete':
      try {
        await fs.unlink(normalized);
        return { success: true, message: '文件已删除' };
      } catch (error) {
        return { error: '删除失败' };
      }
      
    default:
      return { error: '未知操作' };
  }
}
```

---

### 第 4 步：记忆系统

**src/memory.js：**

```javascript
// src/memory.js - 记忆系统

import fs from 'fs/promises';
import path from 'path';

const MEMORY_FILE = './data/memory.json';
const MAX_SHORT_TERM = 10; // 短期记忆容量

// 初始化记忆
export async function initMemory() {
  try {
    await fs.access(MEMORY_FILE);
  } catch {
    await fs.mkdir(path.dirname(MEMORY_FILE), { recursive: true });
    await fs.writeFile(MEMORY_FILE, JSON.stringify({
      shortTerm: [],
      longTerm: [],
      userProfile: {}
    }, null, 2));
  }
}

// 加载记忆
export async function loadMemory() {
  try {
    const data = await fs.readFile(MEMORY_FILE, 'utf-8');
    return JSON.parse(data);
  } catch {
    return { shortTerm: [], longTerm: [], userProfile: {} };
  }
}

// 保存记忆
export async function saveMemory(memory) {
  await fs.writeFile(MEMORY_FILE, JSON.stringify(memory, null, 2));
}

// 添加短期记忆（对话历史）
export async function addShortTermMemory(role, content) {
  const memory = await loadMemory();
  
  memory.shortTerm.push({
    role,
    content,
    timestamp: Date.now()
  });
  
  // 超出容量时，转移到长期记忆
  if (memory.shortTerm.length > MAX_SHORT_TERM) {
    const oldest = memory.shortTerm.shift();
    // 简单实现：可以选择性地保存到长期记忆
    // 实际项目可以用向量数据库做语义检索
  }
  
  await saveMemory(memory);
}

// 获取对话历史（给 LLM 的格式）
export async function getConversationHistory() {
  const memory = await loadMemory();
  return memory.shortTerm.map(m => ({
    role: m.role,
    content: m.content
  }));
}

// 清空短期记忆
export async function clearShortTermMemory() {
  const memory = await loadMemory();
  memory.shortTerm = [];
  await saveMemory(memory);
}

// 更新用户画像
export async function updateUserProfile(key, value) {
  const memory = await loadMemory();
  memory.userProfile[key] = value;
  await saveMemory(memory);
}

// 获取用户画像
export async function getUserProfile() {
  const memory = await loadMemory();
  return memory.userProfile;
}

export default {
  initMemory,
  loadMemory,
  saveMemory,
  addShortTermMemory,
  getConversationHistory,
  clearShortTermMemory,
  updateUserProfile,
  getUserProfile
};
```

---

### 第 5 步：Agent 核心类

**src/agent.js：**

```javascript
// src/agent.js - Agent 核心类

import chalk from 'chalk';
import { chatCompletion } from './llm.js';
import { tools, executeTool } from './tools/index.js';
import { 
  initMemory, 
  addShortTermMemory, 
  getConversationHistory,
  getUserProfile 
} from './memory.js';

class Agent {
  constructor(name = 'AI助手', systemPrompt = '') {
    this.name = name;
    this.systemPrompt = systemPrompt || this.getDefaultSystemPrompt();
  }
  
  getDefaultSystemPrompt() {
    return `你是一个智能任务助手，名叫 ${this.name}。

你的能力：
1. 搜索互联网获取信息
2. 执行数学计算
3. 查询天气
4. 文件操作（读取、写入、列表）

你的特点：
- 友好、专业、高效
- 会主动使用工具解决问题
- 记住用户的偏好和历史对话
- 遇到复杂任务会分解成小步骤

当需要使用工具时，直接调用相应的函数。`;
  }
  
  // 初始化
  async init() {
    await initMemory();
    console.log(chalk.green(`✨ ${this.name} 已启动！`));
    console.log(chalk.gray('输入 "exit" 退出，"clear" 清空对话\n'));
  }
  
  // 处理用户输入
  async processInput(userInput) {
    // 添加用户消息到记忆
    await addShortTermMemory('user', userInput);
    
    // 获取对话历史
    const history = await getConversationHistory();
    const userProfile = await getUserProfile();
    
    // 构建消息
    const messages = [
      { role: 'system', content: this.systemPrompt },
      { role: 'system', content: `用户画像：${JSON.stringify(userProfile)}` },
      ...history
    ];
    
    // 调用 LLM
    let response = await chatCompletion(messages, tools);
    
    // 处理工具调用
    while (response.finish_reason === 'tool_calls') {
      const toolCalls = response.message.tool_calls;
      
      // 执行所有工具
      const toolResults = [];
      for (const call of toolCalls) {
        const result = await executeTool(
          call.function.name,
          JSON.parse(call.function.arguments)
        );
        toolResults.push({
          tool_call_id: call.id,
          role: 'tool',
          content: JSON.stringify(result)
        });
      }
      
      // 添加助手消息和工具结果
      messages.push(response.message);
      messages.push(...toolResults);
      
      // 继续调用 LLM
      response = await chatCompletion(messages, tools);
    }
    
    // 获取最终响应
    const finalResponse = response.message.content;
    
    // 添加助手回复到记忆
    await addShortTermMemory('assistant', finalResponse);
    
    return finalResponse;
  }
}

export default Agent;
```

---

### 第 6 步：主程序

**src/index.js：**

```javascript
// src/index.js - 主程序

import readline from 'readline';
import chalk from 'chalk';
import Agent from './agent.js';
import { clearShortTermMemory } from './memory.js';

// 创建 Agent
const agent = new Agent('小智', `你是一个友好、专业的 AI 助手小智。

你擅长：
- 帮助用户解决问题
- 搜索信息
- 执行计算
- 查询天气

请用简洁、友好的方式回答用户。`);

// 创建命令行界面
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// 提问函数
function question(prompt) {
  return new Promise(resolve => {
    rl.question(prompt, resolve);
  });
}

// 主循环
async function main() {
  await agent.init();
  
  while (true) {
    const userInput = await question(chalk.cyan('你: '));
    
    // 命令处理
    if (userInput.toLowerCase() === 'exit') {
      console.log(chalk.yellow('\n👋 再见！'));
      rl.close();
      break;
    }
    
    if (userInput.toLowerCase() === 'clear') {
      await clearShortTermMemory();
      console.log(chalk.gray('🧹 对话已清空\n'));
      continue;
    }
    
    if (!userInput.trim()) {
      continue;
    }
    
    // 处理输入
    console.log(chalk.gray('\n思考中...\n'));
    
    try {
      const response = await agent.processInput(userInput);
      console.log(chalk.green(`小智: `) + response + '\n');
    } catch (error) {
      console.log(chalk.red(`❌ 错误: ${error.message}\n`));
    }
  }
}

main().catch(console.error);
```

---

## 🧪 测试 Agent

### 运行 Agent

```bash
# 安装依赖
npm install

# 配置环境变量
cp .env.example .env
# 编辑 .env 填入 API Key

# 启动
npm start
```

### 测试对话

```
你: 你好

小智: 你好！我是小智，有什么可以帮你的吗？

你: 今天北京天气怎么样？

🔧 执行工具: get_weather
📝 参数: { city: '北京' }
✅ 结果: { temperature: '15°C', ... }

小智: 北京今天气温 15°C，天气晴朗，湿度 45%...

你: 123 * 456 等于多少？

🔧 执行工具: calculate
📝 参数: { expression: '123 * 456' }
✅ 结果: { result: 56088 }

小智: 123 × 456 = 56088

你: 帮我搜索一下 DeepSeek 最新消息

🔧 执行工具: search
📝 参数: { query: 'DeepSeek 最新消息' }
✅ 结果: { results: [...] }

小智: 根据搜索结果，DeepSeek 最近发布了...
```

---

## 🔄 进阶功能

### 1. 多轮工具调用

Agent 可以连续调用多个工具：

```
用户: 帮我查一下上海天气，然后计算 100 / 5

→ 调用 get_weather({ city: '上海' })
→ 调用 calculate({ expression: '100 / 5' })
→ 综合回复
```

### 2. 任务分解

对于复杂任务，Agent 会自动分解：

```javascript
// 在 system prompt 中添加
当你遇到复杂任务时，请：
1. 将任务分解成小步骤
2. 逐步执行每个步骤
3. 汇总结果给用户
```

### 3. 流式输出

```javascript
// 使用流式 API 改善体验
import { streamChat } from './llm.js';

async function* processWithStream(userInput) {
  for await (const chunk of streamChat(messages)) {
    yield chunk;
  }
}
```

---

## 🐛 常见问题

### Q1: 工具调用失败

**检查：**
- 工具定义格式是否正确
- 参数是否完整
- 网络是否正常

### Q2: 记忆不生效

**检查：**
- memory.json 是否有写入权限
- 是否正确调用 addShortTermMemory

### Q3: API 调用超时

**解决：**
```javascript
// 增加超时时间
const response = await chatCompletion(messages, tools, {
  timeout: 30000
});
```

---

## 📚 进阶学习

### 推荐框架

- **LangChain** - 成熟的 Agent 框架
- **AutoGPT** - 自主 Agent 实现
- **CrewAI** - 多 Agent 协作框架
- **Semantic Kernel** - 微软的 AI 编排框架

### 学习路径

1. **入门**：实现基础 Agent（本项目）
2. **进阶**：添加向量数据库记忆
3. **高级**：多 Agent 协作系统
4. **专业**：自主 Agent（AutoGPT 风格）

---

## ✅ 学习检查

完成本项目后，你应该掌握了：

- [ ] Agent 核心架构
- [ ] Function Calling 工具调用
- [ ] 记忆系统设计
- [ ] 多轮对话处理
- [ ] 错误处理和调试

---

**💡 小贴士：** Agent 是 AI 应用的未来形态。掌握 Agent 开发，你就掌握了构建智能应用的核心技能！

---

## 📚 相关章节

- [项目实战目录](./README.md)
- [Web 应用部署](./08-Web部署.md)
- [MCP 开发](../../05-MCP开发.md)
