# MCP开发

> 让AI连接真实世界的桥梁

---

## 🤔 什么是MCP？

**MCP = Model Context Protocol = 模型上下文协议**

简单来说，MCP是一个标准协议，让Claude能够连接外部工具、数据库、API，实现真正的"AI Agent"。

**MCP的核心价值：**
- 🔗 **连接外部世界** - AI不再只是聊天，能执行真实操作
- 📊 **访问私有数据** - 连接你的数据库、文件、API
- 🔧 **扩展能力** - 给AI添加任何你需要的功能
- 🎯 **标准化** - 一次开发，到处使用

**类比理解：**
- **没有MCP** = AI是个"闭关锁国"的智者，只能聊天
- **有了MCP** = AI变成了"全能助手"，能帮你查数据、发邮件、操作文件

---

## 🌟 MCP能做什么？

### 1️⃣ 连接数据源

**✅ 支持的数据源：**
- 📁 **本地文件** - 读取/写入文件
- 🗄️ **数据库** - PostgreSQL, MySQL, SQLite
- 🔍 **搜索引擎** - Brave Search, Google
- 📊 **数据分析** - Pandas, Excel
- ☁️ **云服务** - AWS, GCP, Azure

**示例：**

```
用户：帮我分析去年的销售数据

Claude（通过MCP）：
1. 连接PostgreSQL数据库
2. 执行SQL查询
3. 用Pandas分析数据
4. 生成可视化图表
5. 返回分析报告
```

---

### 2️⃣ 调用外部API

**✅ 支持的API：**
- 📧 **邮件** - Gmail, Outlook
- 📱 **消息** - Slack, Discord, Telegram
- 🗓️ **日历** - Google Calendar, Outlook
- 📝 **文档** - Notion, Google Docs
- 🐙 **开发** - GitHub, GitLab

**示例：**

```
用户：帮我发送周报邮件给团队

Claude（通过MCP）：
1. 连接Gmail API
2. 生成周报内容
3. 获取团队成员邮箱
4. 发送邮件
5. 返回发送结果
```

---

### 3️⃣ 执行系统操作

**✅ 支持的操作：**
- 💻 **命令执行** - 运行shell命令
- 📦 **包管理** - 安装/更新软件
- 🔄 **自动化** - 定时任务、工作流
- 🐳 **容器** - Docker操作
- ☸️ **编排** - Kubernetes管理

**示例：**

```
用户：帮我部署这个应用到生产环境

Claude（通过MCP）：
1. 构建Docker镜像
2. 推送到镜像仓库
3. 更新Kubernetes部署
4. 验证服务状态
5. 返回部署结果
```

---

## 🛠️ MCP开发入门

### 1️⃣ MCP架构

**三个核心组件：**

```
┌─────────────┐
│   Claude    │  AI模型
└──────┬──────┘
       │
       ↓
┌─────────────┐
│  MCP Client │  客户端（Claude Desktop / Claude Code）
└──────┬──────┘
       │
       ↓
┌─────────────┐
│  MCP Server │  服务器（你开发的服务）
└──────┬──────┘
       │
       ↓
┌─────────────┐
│  Resources  │  资源（数据库、API、文件）
└─────────────┘
```

---

### 2️⃣ 开发你的第一个MCP Server

**需求：** 创建一个简单的文件管理MCP Server

**步骤：**

#### 第一步：安装依赖

```bash
# 创建项目目录
mkdir my-mcp-server
cd my-mcp-server

# 初始化项目
npm init -y

# 安装MCP SDK
npm install @modelcontextprotocol/sdk
```

---

#### 第二步：编写Server代码

**创建 `server.js`：**

```javascript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import fs from "fs/promises";
import path from "path";

// 创建MCP Server
const server = new Server(
  {
    name: "file-manager",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},  // 提供工具
      resources: {},  // 提供资源
    },
  }
);

// 定义工具列表
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "read_file",
        description: "读取文件内容",
        inputSchema: {
          type: "object",
          properties: {
            path: {
              type: "string",
              description: "文件路径",
            },
          },
          required: ["path"],
        },
      },
      {
        name: "write_file",
        description: "写入文件内容",
        inputSchema: {
          type: "object",
          properties: {
            path: {
              type: "string",
              description: "文件路径",
            },
            content: {
              type: "string",
              description: "文件内容",
            },
          },
          required: ["path", "content"],
        },
      },
      {
        name: "list_files",
        description: "列出目录下的文件",
        inputSchema: {
          type: "object",
          properties: {
            path: {
              type: "string",
              description: "目录路径",
            },
          },
          required: ["path"],
        },
      },
    ],
  };
});

// 实现工具逻辑
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;

  switch (name) {
    case "read_file": {
      const content = await fs.readFile(args.path, "utf-8");
      return {
        content: [{ type: "text", text: content }],
      };
    }

    case "write_file": {
      await fs.writeFile(args.path, args.content, "utf-8");
      return {
        content: [{ type: "text", text: `文件已保存: ${args.path}` }],
      };
    }

    case "list_files": {
      const files = await fs.readdir(args.path);
      return {
        content: [{ type: "text", text: files.join("\n") }],
      };
    }

    default:
      throw new Error(`未知工具: ${name}`);
  }
});

// 启动服务器
const transport = new StdioServerTransport();
await server.connect(transport);
```

---

#### 第三步：配置Claude Desktop

**打开配置文件：**

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

**添加MCP Server配置：**

```json
{
  "mcpServers": {
    "file-manager": {
      "command": "node",
      "args": ["/path/to/my-mcp-server/server.js"]
    }
  }
}
```

---

#### 第四步：重启Claude Desktop

**重启后，Claude就能使用你的MCP Server了！**

**测试：**

```
用户：帮我读取 /Users/cgz/test.txt 文件

Claude（通过MCP）：
- 调用 read_file 工具
- 返回文件内容
```

---

## 🎯 MCP开发最佳实践

### 1️⃣ 工具设计原则

**✅ 好的工具设计：**

```javascript
{
  name: "search_database",  // 清晰的名字
  description: "搜索PostgreSQL数据库中的用户信息",  // 详细描述
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "SQL查询语句（SELECT only）",  // 参数说明
      },
      limit: {
        type: "number",
        description: "返回结果数量限制（默认100）",
        default: 100,
      },
    },
    required: ["query"],  // 必填参数
  },
}
```

**❌ 不好的设计：**

```javascript
{
  name: "db",  // 名字不清楚
  description: "数据库操作",  // 描述太模糊
  inputSchema: {
    type: "object",
    properties: {
      q: { type: "string" },  // 没有描述
    },
  },
}
```

---

### 2️⃣ 安全考虑

**⚠️ 安全原则：**

1. **输入验证**
   ```javascript
   // 验证文件路径
   if (args.path.includes("..")) {
     throw new Error("不允许访问上级目录");
   }
   ```

2. **权限控制**
   ```javascript
   // 只允许访问特定目录
   const allowedPath = "/Users/cgz/data";
   if (!args.path.startsWith(allowedPath)) {
     throw new Error("无权访问此目录");
   }
   ```

3. **敏感信息保护**
   ```javascript
   // 过滤敏感字段
   const sanitizeResult = (data) => {
     const { password, token, ...safe } = data;
     return safe;
   };
   ```

---

### 3️⃣ 错误处理

**✅ 完善的错误处理：**

```javascript
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  try {
    const { name, arguments: args } = request.params;

    switch (name) {
      case "read_file": {
        // 检查文件是否存在
        try {
          await fs.access(args.path);
        } catch {
          throw new Error(`文件不存在: ${args.path}`);
        }

        // 读取文件
        const content = await fs.readFile(args.path, "utf-8");
        return {
          content: [{ type: "text", text: content }],
        };
      }

      default:
        throw new Error(`未知工具: ${name}`);
    }
  } catch (error) {
    // 返回友好的错误信息
    return {
      content: [
        {
          type: "text",
          text: `❌ 错误: ${error.message}`,
        },
      ],
      isError: true,
    };
  }
});
```

---

## 📚 实战案例

### 案例1：数据库MCP Server

**需求：** 让Claude能查询PostgreSQL

**server.js：**

```javascript
import { Client } from "pg";

const pgClient = new Client({
  connectionString: process.env.DATABASE_URL,
});

await pgClient.connect();

// 定义工具
{
  name: "query_database",
  description: "执行SQL查询（SELECT only）",
  inputSchema: {
    type: "object",
    properties: {
      sql: {
        type: "string",
        description: "SELECT查询语句",
      },
    },
    required: ["sql"],
  },
}

// 实现逻辑
case "query_database": {
  // 只允许SELECT
  if (!args.sql.trim().toLowerCase().startsWith("select")) {
    throw new Error("只允许SELECT查询");
  }

  const result = await pgClient.query(args.sql);
  return {
    content: [{ type: "text", text: JSON.stringify(result.rows, null, 2) }],
  };
}
```

**使用：**

```
用户：查询最近7天的订单数据

Claude（通过MCP）：
- 执行 SELECT * FROM orders WHERE created_at > NOW() - INTERVAL '7 days'
- 返回结果
```

---

### 案例2：GitHub MCP Server

**需求：** 让Claude能操作GitHub

**server.js：**

```javascript
import { Octokit } from "octokit";

const octokit = new Octokit({
  auth: process.env.GITHUB_TOKEN,
});

// 定义工具
{
  name: "create_issue",
  description: "创建GitHub Issue",
  inputSchema: {
    type: "object",
    properties: {
      owner: { type: "string", description: "仓库所有者" },
      repo: { type: "string", description: "仓库名称" },
      title: { type: "string", description: "Issue标题" },
      body: { type: "string", description: "Issue内容" },
    },
    required: ["owner", "repo", "title"],
  },
}

// 实现逻辑
case "create_issue": {
  const issue = await octokit.rest.issues.create({
    owner: args.owner,
    repo: args.repo,
    title: args.title,
    body: args.body,
  });

  return {
    content: [
      {
        type: "text",
        text: `Issue已创建: ${issue.data.html_url}`,
      },
    ],
  };
}
```

**使用：**

```
用户：在 openclaw/openclaw 仓库创建一个Bug Issue

Claude（通过MCP）：
- 调用 create_issue 工具
- 返回Issue链接
```

---

## 🚀 MCP生态系统

### 官方MCP Servers

**Anthropic提供的官方Server：**

| Server | 功能 |
|--------|------|
| filesystem | 文件系统操作 |
| postgres | PostgreSQL数据库 |
| sqlite | SQLite数据库 |
| brave-search | Brave搜索 |
| google-search | Google搜索 |
| github | GitHub操作 |
| gitlab | GitLab操作 |
| slack | Slack集成 |
| puppeteer | 浏览器自动化 |

**安装方式：**

```bash
# 克隆官方MCP Servers
git clone https://github.com/modelcontextprotocol/servers.git

# 安装依赖
cd servers
npm install

# 配置到Claude Desktop
# 编辑 claude_desktop_config.json
```

---

### 社区MCP Servers

**热门社区Server：**

| Server | 功能 | Stars |
|--------|------|-------|
| mcp-server-notion | Notion集成 | 500+ |
| mcp-server-discord | Discord集成 | 300+ |
| mcp-server-kubernetes | K8s管理 | 200+ |
| mcp-server-aws | AWS集成 | 150+ |

**查找更多：**
- GitHub搜索：`topic:mcp-server`
- 官方列表：[modelcontextprotocol.io/servers](https://modelcontextprotocol.io/servers)

---

## ❓ 常见问题

<details>
<summary><b>Q: MCP Server用什么语言开发？</b></summary>

**A: 任何语言都可以！**

- ✅ TypeScript/JavaScript（官方SDK）
- ✅ Python（社区SDK）
- ✅ Go（社区SDK）
- ✅ 任何支持stdio的语言

**推荐：** TypeScript + 官方SDK
</details>

<details>
<summary><b>Q: MCP Server安全吗？</b></summary>

**A: 取决于你的实现。**

**需要注意：**
1. 输入验证（防止注入攻击）
2. 权限控制（限制访问范围）
3. 敏感信息（不要暴露密码/token）
4. 沙箱隔离（生产环境用容器）

**建议：** 最小权限原则
</details>

<details>
<summary><b>Q: 如何调试MCP Server？</b></summary>

**A: 使用日志和测试工具。**

```javascript
// 添加日志
console.error("[MCP] 收到请求:", request);

// 使用MCP Inspector
npx @modelcontextprotocol/inspector node server.js
```

**测试：** Claude Desktop → 开发者工具 → Console
</details>

<details>
<summary><b>Q: MCP会开源吗？</b></summary>

**A: 已经开源！**

- 协议规范：MIT License
- SDK：MIT License
- 官方Servers：MIT License

**仓库：** [github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)
</details>

---

## 🎯 下一步

学完MCP开发，接下来：

👉 [VibeCoding入门](./01-VibeCoding入门.md) - 回顾基础

👉 [Claude Code技巧](./03-ClaudeCode技巧.md) - 深入Claude

---

**💡 小贴士：** MCP的核心是"标准化"，一次开发，所有支持MCP的AI都能用！
