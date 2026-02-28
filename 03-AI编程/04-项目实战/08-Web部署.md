# 08-Web 应用部署

> 让你的项目上线，全世界都能访问

---

## 📖 项目介绍

开发完项目后，最重要的就是部署上线。本教程教你如何把本地项目发布到互联网，让全世界都能访问。

**难度：** ⭐⭐⭐⭐（中高级）

**你将学到：**
- 多种免费部署平台
- 域名和 DNS 配置
- HTTPS 证书配置
- CI/CD 自动化部署

---

## 🎯 部署平台对比

| 平台 | 特点 | 免费额度 | 适合项目 |
|------|------|----------|----------|
| **GitHub Pages** | 简单、免费、支持自定义域名 | 100GB/月 | 静态网站 |
| **Vercel** | Next.js 官方、自动部署 | 100GB/月 | React/Next.js |
| **Netlify** | 功能丰富、表单处理 | 100GB/月 | 静态网站 |
| **Cloudflare Pages** | 全球 CDN、速度快 | 无限带宽 | 静态网站 |
| **Railway** | 支持后端、数据库 | $5/月额度 | 全栈应用 |
| **Render** | 免费 Web 服务 | 750小时/月 | Node.js/Python |

---

## 📁 部署前准备

### 1. 项目结构整理

**标准项目结构：**
```
my-project/
├── index.html          # 入口文件
├── css/
│   └── style.css
├── js/
│   └── app.js
├── images/
│   └── logo.png
├── package.json        # Node.js 项目
├── .gitignore          # Git 忽略文件
└── README.md           # 项目说明
```

### 2. 创建 .gitignore

**向 AI 提问：**
```
帮我创建一个 .gitignore 文件，要求：
- 忽略 node_modules
- 忽略 .env 环境变量文件
- 忽略打包后的 dist/build 目录
- 忽略编辑器配置文件
- 忽略系统文件（.DS_Store）
```

**生成的 .gitignore：**
```gitignore
# Dependencies
node_modules/

# Build output
dist/
build/
.next/

# Environment variables
.env
.env.local
.env.*.local

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*

# Test
coverage/
```

### 3. 初始化 Git

```bash
# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 提交
git commit -m "Initial commit"

# 关联远程仓库
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

# 推送到 GitHub
git push -u origin main
```

---

## 🚀 GitHub Pages 部署

最简单的免费部署方式，适合静态网站。

### 方法一：直接部署

**步骤：**
1. 在 GitHub 创建仓库
2. 上传项目文件
3. 进入仓库 Settings → Pages
4. Source 选择 `main` 分支
5. 点击 Save
6. 等待几分钟后访问 `https://YOUR_USERNAME.github.io/YOUR_REPO`

### 方法二：GitHub Actions 自动部署

**创建 .github/workflows/deploy.yml：**

**向 AI 提问：**
```
帮我创建一个 GitHub Actions 配置文件，要求：
- 当推送到 main 分支时自动部署
- 使用 Node.js 18
- 运行 npm install 和 npm run build
- 部署到 GitHub Pages
```

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build
        
      - name: Setup Pages
        uses: actions/configure-pages@v4
        
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './dist'
          
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
```

### 自定义域名

**步骤：**
1. 在项目根目录创建 `CNAME` 文件
2. 文件内容写入你的域名（如 `www.example.com`）
3. 在域名服务商添加 DNS 记录：
   - 类型：CNAME
   - 名称：www
   - 值：YOUR_USERNAME.github.io
4. 等待 DNS 生效（可能需要几分钟到几小时）

---

## 🔥 Vercel 部署

最适合 React/Next.js 项目，自动部署、性能优秀。

### 快速部署

**步骤：**
1. 访问 [vercel.com](https://vercel.com)
2. 使用 GitHub 账号登录
3. 点击「Import Project」
4. 选择你的 GitHub 仓库
5. 点击「Deploy」
6. 等待部署完成

### 配置文件 vercel.json

**向 AI 提问：**
```
帮我创建一个 vercel.json 配置文件，要求：
- 配置 SPA 路由重定向
- 设置缓存策略
- 配置自定义域名
```

```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ],
  "domains": ["your-domain.com"]
}
```

### 环境变量配置

1. 在 Vercel 项目设置中
2. 进入 Environment Variables
3. 添加变量：
   - `API_KEY` = `your_api_key`
   - `API_URL` = `https://api.example.com`
4. 重新部署生效

---

## 🌐 Netlify 部署

功能最丰富的静态网站托管平台。

### 快速部署

**步骤：**
1. 访问 [netlify.com](https://netlify.com)
2. 使用 GitHub 登录
3. 点击「Add new site」→「Import an existing project」
4. 选择 GitHub 仓库
5. 配置构建设置：
   - Build command: `npm run build`
   - Publish directory: `dist`
6. 点击「Deploy site」

### 配置文件 netlify.toml

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    Content-Security-Policy = "default-src 'self'"

[build.environment]
  NODE_VERSION = "18"
```

### Netlify Forms 表单处理

**HTML 中添加：**
```html
<form name="contact" method="POST" data-netlify="true">
  <input type="text" name="name" required>
  <input type="email" name="email" required>
  <textarea name="message" required></textarea>
  <button type="submit">发送</button>
</form>
```

Netlify 会自动处理表单提交，在后台查看数据。

---

## ⚡ Cloudflare Pages 部署

全球 CDN，速度最快，无限带宽。

### 快速部署

**步骤：**
1. 访问 [pages.cloudflare.com](https://pages.cloudflare.com)
2. 连接 GitHub 账号
3. 选择仓库
4. 配置构建：
   - Build command: `npm run build`
   - Build output: `dist`
5. 点击「Save and Deploy」

### 配置文件 wrangler.toml

```toml
name = "my-project"
compatibility_date = "2024-01-01"

[site]
bucket = "./dist"

[[redirects]]
from = "/*"
to = "/index.html"
status = 200
```

---

## 🚂 Railway 部署（后端项目）

支持 Node.js、Python、数据库的全栈部署平台。

### 部署 Node.js 后端

**项目结构：**
```
my-api/
├── server.js
├── package.json
├── .env.example
└── Procfile
```

**Procfile：**
```
web: node server.js
```

**server.js 示例：**
```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.json({ message: 'Hello from Railway!' });
});

app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ]);
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

**部署步骤：**
1. 访问 [railway.app](https://railway.app)
2. 使用 GitHub 登录
3. 点击「New Project」
4. 选择「Deploy from GitHub repo」
5. 选择仓库
6. 添加环境变量
7. 等待部署

### 添加数据库

1. 在项目中点击「+」
2. 选择数据库类型（PostgreSQL/MySQL/Redis）
3. 自动生成连接字符串
4. 在代码中使用环境变量：

```javascript
const { Pool } = require('pg');
const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});
```

---

## 🐳 Docker 部署

适合需要完全控制部署环境的场景。

### 创建 Dockerfile

**向 AI 提问：**
```
帮我创建一个 Dockerfile，要求：
- 使用 Node.js 18 Alpine 镜像
- 复制 package.json 并安装依赖
- 复制项目文件
- 构建项目
- 使用 nginx 服务静态文件
- 暴露 80 端口
```

```dockerfile
# 构建阶段
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# 生产阶段
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### nginx.conf 配置

```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 静态资源缓存
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;
}
```

### 构建和运行

```bash
# 构建镜像
docker build -t my-app .

# 运行容器
docker run -d -p 80:80 my-app

# 访问 http://localhost
```

---

## 🔄 CI/CD 自动化部署

### GitHub Actions 完整配置

**.github/workflows/deploy.yml：**

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run tests
        run: npm test
        
      - name: Build
        run: npm run build
        
      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Download build artifacts
        uses: actions/download-artifact@v4
        with:
          name: build
          path: dist/
          
      - name: Deploy to Server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/my-app
            git pull origin main
            npm ci
            npm run build
            pm2 restart all
```

---

## 🌍 自定义域名配置

### 购买域名

推荐域名服务商：
- **国内**：阿里云、腾讯云
- **国外**：Namecheap、Cloudflare、GoDaddy

### DNS 配置

**常用记录类型：**

| 类型 | 用途 | 示例 |
|------|------|------|
| A | 指向 IP 地址 | @ → 192.0.2.1 |
| CNAME | 指向另一个域名 | www → example.com |
| MX | 邮件服务器 | @ → mail.example.com |
| TXT | 文本记录（验证用） | @ → v=spf1... |

### 配置 HTTPS

**免费 SSL 证书：**
1. **Let's Encrypt** - 免费自动续期
2. **Cloudflare** - 免费 SSL 代理
3. **平台自带** - Vercel/Netlify 自动配置

---

## 📊 性能优化

### 1. 静态资源优化

**向 AI 提问：**
```
帮我优化 Vite 构建配置，要求：
- 代码分割
- 压缩 JS/CSS
- 图片压缩
- 生成 hash 文件名
```

**vite.config.js：**
```javascript
import { defineConfig } from 'vite';
import viteCompression from 'vite-plugin-compression';

export default defineConfig({
  build: {
    // 代码分割
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          utils: ['lodash', 'axios']
        }
      }
    },
    // 压缩
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true
      }
    },
    // hash 文件名
    rollupOptions: {
      output: {
        entryFileNames: 'assets/[name].[hash].js',
        chunkFileNames: 'assets/[name].[hash].js',
        assetFileNames: 'assets/[name].[hash].[ext]'
      }
    }
  },
  plugins: [
    // Gzip 压缩
    viteCompression()
  ]
});
```

### 2. CDN 加速

使用 Cloudflare CDN：
1. 添加网站到 Cloudflare
2. 更新域名 DNS 到 Cloudflare
3. 开启 CDN 代理
4. 配置缓存规则

### 3. 性能监控

**Lighthouse 评分检查：**
```bash
# 安装 Lighthouse
npm install -g lighthouse

# 运行检测
lighthouse https://your-site.com --view
```

---

## 🐛 常见问题

### Q1: 部署后页面空白

**可能原因：**
- 路由配置问题
- 静态资源路径错误
- 环境变量未配置

**解决方法：**
```javascript
// vite.config.js 添加 base
export default defineConfig({
  base: '/your-repo-name/',  // GitHub Pages 需要
  // ...
});
```

---

### Q2: API 请求跨域

**解决方法：**

Vercel 配置代理：
```json
// vercel.json
{
  "rewrites": [
    {
      "source": "/api/:path*",
      "destination": "https://api.example.com/:path*"
    }
  ]
}
```

---

### Q3: 部署很慢

**优化方法：**
- 使用 npm ci 代替 npm install
- 缓存 node_modules
- 减少依赖包数量
- 使用 pnpm 代替 npm

---

### Q4: 环境变量泄露

**安全检查：**
- 不要在前端代码中使用敏感 API Key
- 使用 .env.local 存储本地环境变量
- 确保 .gitignore 包含 .env 文件
- 在部署平台配置环境变量

---

## ✅ 部署检查清单

部署前确认：

- [ ] 代码已提交到 Git
- [ ] .gitignore 配置正确
- [ ] 敏感信息不在代码中
- [ ] 构建命令正确
- [ ] 环境变量已配置
- [ ] 域名 DNS 已配置
- [ ] HTTPS 证书正常
- [ ] 页面可正常访问
- [ ] API 请求正常
- [ ] 性能测试通过

---

## 📚 进阶学习

### 推荐资源

- [GitHub Pages 文档](https://docs.github.com/pages)
- [Vercel 文档](https://vercel.com/docs)
- [Netlify 文档](https://docs.netlify.com)
- [Cloudflare Pages 文档](https://developers.cloudflare.com/pages)

### 学习路径

1. **入门**：GitHub Pages 部署静态网站
2. **进阶**：Vercel/Netlify 自动化部署
3. **高级**：Docker + 云服务器部署
4. **专业**：Kubernetes 容器编排

---

**💡 小贴士：** 部署不是终点，而是起点。持续监控、优化、迭代，让你的应用越来越好！

---

## 📚 相关章节

- [项目实战目录](./README.md)
- [Chrome 扩展开发](./07-Chrome扩展.md)
- [AI Agent 开发](./09-AI-Agent.md)
