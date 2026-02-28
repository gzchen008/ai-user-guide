# 07-Chrome 扩展开发

> 打造属于你的浏览器神器

---

## 📖 项目介绍

Chrome 扩展可以增强浏览器功能，比如广告屏蔽、网页翻译、截图工具等。本教程带你用 AI 开发一个实用的 Chrome 扩展。

**难度：** ⭐⭐⭐⭐（中高级）

**你将学到：**
- Chrome 扩展架构和配置
- 内容脚本和后台脚本
- 浏览器 API 使用
- 扩展发布流程

---

## 🎯 项目目标

我们要开发一个 **「网页高亮笔记」** 扩展：

**核心功能：**
- 在任意网页上选中文本高亮标记
- 添加笔记注释
- 保存高亮记录
- 支持多种高亮颜色

**为什么选这个项目？**
- 实用性强，自己能用
- 涵盖扩展开发核心知识
- 难度适中，适合进阶

---

## 📁 项目结构

```
highlight-extension/
├── manifest.json          # 扩展配置文件
├── popup.html             # 弹出窗口界面
├── popup.css              # 弹出窗口样式
├── popup.js               # 弹出窗口逻辑
├── content.js             # 注入网页的脚本
├── content.css            # 注入网页的样式
├── background.js          # 后台服务脚本
├── icons/                 # 扩展图标
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
└── README.md              # 说明文档
```

---

## 🚀 开发步骤

### 第 1 步：创建项目结构

**向 AI 提问：**
```
帮我创建一个 Chrome 扩展项目，名称是「网页高亮笔记」。
请创建完整的项目目录结构，包括：
- manifest.json（Manifest V3）
- popup 界面文件
- content script 文件
- background script 文件
- icons 目录
```

---

### 第 2 步：配置 manifest.json

这是扩展的「身份证」，告诉浏览器你的扩展信息。

**向 AI 提问：**
```
帮我创建 manifest.json 文件，要求：
- 使用 Manifest V3 版本
- 扩展名称：网页高亮笔记
- 版本号：1.0.0
- 需要的权限：activeTab, storage
- 包含 content_scripts 和 background service_worker
```

**AI 生成的 manifest.json：**
```json
{
  "manifest_version": 3,
  "name": "网页高亮笔记",
  "version": "1.0.0",
  "description": "在网页上高亮文本并添加笔记",
  
  "permissions": [
    "activeTab",
    "storage"
  ],
  
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icons/icon16.png",
      "48": "icons/icon48.png",
      "128": "icons/icon128.png"
    }
  },
  
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"],
      "css": ["content.css"]
    }
  ],
  
  "background": {
    "service_worker": "background.js"
  }
}
```

**关键配置说明：**
- `manifest_version: 3` - 使用最新版本
- `permissions` - 需要的权限
- `action` - 点击扩展图标弹出的界面
- `content_scripts` - 注入到网页的脚本
- `background` - 后台服务

---

### 第 3 步：创建弹出窗口

**向 AI 提问：**
```
帮我创建 popup.html 和 popup.css，要求：
- 现代简洁的界面设计
- 包含高亮颜色选择（黄色、绿色、蓝色、粉色）
- 包含「清除所有高亮」按钮
- 包含「查看笔记列表」按钮
- 宽度 300px，居中显示
```

**popup.html：**
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      width: 300px;
      padding: 16px;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background: #f8f9fa;
    }
    
    .header {
      text-align: center;
      margin-bottom: 16px;
    }
    
    .header h1 {
      font-size: 18px;
      color: #333;
      margin-bottom: 4px;
    }
    
    .header p {
      font-size: 12px;
      color: #666;
    }
    
    .color-picker {
      display: flex;
      gap: 12px;
      justify-content: center;
      margin-bottom: 16px;
    }
    
    .color-btn {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      border: 3px solid transparent;
      cursor: pointer;
      transition: all 0.2s;
    }
    
    .color-btn:hover {
      transform: scale(1.1);
    }
    
    .color-btn.active {
      border-color: #333;
    }
    
    .color-yellow { background: #ffeb3b; }
    .color-green { background: #4caf50; }
    .color-blue { background: #2196f3; }
    .color-pink { background: #e91e63; }
    
    .actions {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    
    .btn {
      padding: 12px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 500;
      transition: all 0.2s;
    }
    
    .btn-primary {
      background: #4285f4;
      color: white;
    }
    
    .btn-primary:hover {
      background: #3367d6;
    }
    
    .btn-danger {
      background: #ea4335;
      color: white;
    }
    
    .btn-danger:hover {
      background: #c5221f;
    }
    
    .stats {
      margin-top: 16px;
      padding: 12px;
      background: white;
      border-radius: 8px;
      text-align: center;
    }
    
    .stats p {
      font-size: 12px;
      color: #666;
    }
    
    .stats .count {
      font-size: 24px;
      font-weight: bold;
      color: #4285f4;
    }
  </style>
</head>
<body>
  <div class="header">
    <h1>🖍️ 网页高亮笔记</h1>
    <p>选中文本后点击颜色高亮</p>
  </div>
  
  <div class="color-picker">
    <button class="color-btn color-yellow active" data-color="yellow"></button>
    <button class="color-btn color-green" data-color="green"></button>
    <button class="color-btn color-blue" data-color="blue"></button>
    <button class="color-btn color-pink" data-color="pink"></button>
  </div>
  
  <div class="actions">
    <button class="btn btn-primary" id="viewNotes">📝 查看笔记列表</button>
    <button class="btn btn-danger" id="clearAll">🗑️ 清除所有高亮</button>
  </div>
  
  <div class="stats">
    <p>本页高亮数</p>
    <div class="count" id="highlightCount">0</div>
  </div>
  
  <script src="popup.js"></script>
</body>
</html>
```

---

### 第 4 步：实现弹出窗口逻辑

**popup.js：**
```javascript
// popup.js - 弹出窗口逻辑

document.addEventListener('DOMContentLoaded', function() {
  let selectedColor = 'yellow';
  
  // 颜色选择
  document.querySelectorAll('.color-btn').forEach(btn => {
    btn.addEventListener('click', function() {
      // 移除其他选中状态
      document.querySelectorAll('.color-btn').forEach(b => b.classList.remove('active'));
      // 添加选中状态
      this.classList.add('active');
      selectedColor = this.dataset.color;
      
      // 保存选中颜色
      chrome.storage.local.set({ selectedColor });
    });
  });
  
  // 查看笔记列表
  document.getElementById('viewNotes').addEventListener('click', function() {
    // 发送消息给 content script
    chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
      chrome.tabs.sendMessage(tabs[0].id, { action: 'viewNotes' });
    });
    // 关闭弹出窗口
    window.close();
  });
  
  // 清除所有高亮
  document.getElementById('clearAll').addEventListener('click', function() {
    if (confirm('确定要清除本页所有高亮吗？')) {
      chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
        chrome.tabs.sendMessage(tabs[0].id, { action: 'clearAll' });
      });
      // 更新计数
      document.getElementById('highlightCount').textContent = '0';
    }
  });
  
  // 加载选中颜色
  chrome.storage.local.get(['selectedColor'], function(result) {
    if (result.selectedColor) {
      selectedColor = result.selectedColor;
      document.querySelectorAll('.color-btn').forEach(btn => {
        btn.classList.toggle('active', btn.dataset.color === selectedColor);
      });
    }
  });
  
  // 获取高亮数量
  chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
    chrome.tabs.sendMessage(tabs[0].id, { action: 'getCount' }, function(response) {
      if (response && response.count !== undefined) {
        document.getElementById('highlightCount').textContent = response.count;
      }
    });
  });
});
```

---

### 第 5 步：实现内容脚本

**content.js - 核心功能：**
```javascript
// content.js - 注入到网页的脚本

(function() {
  'use strict';
  
  // 高亮颜色映射
  const colorMap = {
    yellow: '#ffeb3b',
    green: '#4caf50',
    blue: '#2196f3',
    pink: '#e91e63'
  };
  
  // 高亮数据存储
  let highlights = [];
  
  // 初始化：从 storage 加载高亮数据
  function init() {
    loadHighlights();
    createContextMenu();
    createNotePanel();
  }
  
  // 加载已保存的高亮
  function loadHighlights() {
    const url = window.location.href;
    chrome.storage.local.get([url], function(result) {
      if (result[url]) {
        highlights = result[url];
        highlights.forEach(h => applyHighlight(h));
      }
    });
  }
  
  // 保存高亮数据
  function saveHighlights() {
    const url = window.location.href;
    chrome.storage.local.set({ [url]: highlights });
  }
  
  // 应用高亮
  function applyHighlight(highlight) {
    const selection = window.getSelection();
    // 使用 CSS 类标记高亮
    const range = document.createRange();
    // 实际实现需要更复杂的 DOM 操作
  }
  
  // 创建右键菜单
  function createContextMenu() {
    document.addEventListener('contextmenu', function(e) {
      const selection = window.getSelection();
      if (selection.toString().trim()) {
        // 显示高亮选项
        showHighlightMenu(e.pageX, e.pageY);
      }
    });
  }
  
  // 显示高亮菜单
  function showHighlightMenu(x, y) {
    // 移除已存在的菜单
    const existing = document.getElementById('highlight-menu');
    if (existing) existing.remove();
    
    // 创建菜单
    const menu = document.createElement('div');
    menu.id = 'highlight-menu';
    menu.innerHTML = `
      <div class="highlight-menu-item" data-color="yellow">🟡 黄色高亮</div>
      <div class="highlight-menu-item" data-color="green">🟢 绿色高亮</div>
      <div class="highlight-menu-item" data-color="blue">🔵 蓝色高亮</div>
      <div class="highlight-menu-item" data-color="pink">🩷 粉色高亮</div>
      <div class="highlight-menu-item" data-action="note">📝 添加笔记</div>
    `;
    menu.style.cssText = `
      position: absolute;
      left: ${x}px;
      top: ${y}px;
      background: white;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      padding: 8px 0;
      z-index: 10000;
    `;
    
    document.body.appendChild(menu);
    
    // 点击高亮选项
    menu.querySelectorAll('.highlight-menu-item').forEach(item => {
      item.addEventListener('click', function() {
        const color = this.dataset.color;
        if (color) {
          highlightSelection(color);
        } else if (this.dataset.action === 'note') {
          addNote();
        }
        menu.remove();
      });
    });
    
    // 点击其他地方关闭菜单
    document.addEventListener('click', function closeMenu() {
      menu.remove();
      document.removeEventListener('click', closeMenu);
    }, { once: true });
  }
  
  // 高亮选中文字
  function highlightSelection(color) {
    const selection = window.getSelection();
    if (!selection.rangeCount) return;
    
    const range = selection.getRangeAt(0);
    const selectedText = selection.toString();
    
    // 创建高亮 span
    const span = document.createElement('span');
    span.className = 'web-highlight';
    span.style.backgroundColor = colorMap[color];
    span.style.cursor = 'pointer';
    span.dataset.highlightId = Date.now().toString();
    
    // 包裹选中内容
    range.surroundContents(span);
    
    // 保存高亮数据
    const highlight = {
      id: span.dataset.highlightId,
      text: selectedText,
      color: color,
      url: window.location.href,
      timestamp: Date.now()
    };
    highlights.push(highlight);
    saveHighlights();
    
    // 清除选择
    selection.removeAllRanges();
    
    // 点击高亮显示笔记
    span.addEventListener('click', function(e) {
      e.stopPropagation();
      showHighlightNote(this);
    });
  }
  
  // 添加笔记
  function addNote() {
    const note = prompt('请输入笔记内容：');
    if (note) {
      // 保存笔记
      console.log('Note added:', note);
    }
  }
  
  // 创建笔记面板
  function createNotePanel() {
    const panel = document.createElement('div');
    panel.id = 'note-panel';
    panel.style.cssText = `
      position: fixed;
      right: 20px;
      top: 100px;
      width: 300px;
      max-height: 400px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.15);
      padding: 16px;
      z-index: 10000;
      display: none;
      overflow-y: auto;
    `;
    document.body.appendChild(panel);
  }
  
  // 显示笔记面板
  function showNotePanel() {
    const panel = document.getElementById('note-panel');
    panel.innerHTML = `
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
        <h3 style="margin: 0;">📝 高亮笔记</h3>
        <button id="close-panel" style="border: none; background: none; font-size: 20px; cursor: pointer;">×</button>
      </div>
      <div class="notes-list">
        ${highlights.map(h => `
          <div class="note-item" style="padding: 12px; background: #f8f9fa; border-radius: 8px; margin-bottom: 8px;">
            <div style="font-size: 14px; margin-bottom: 4px;">${h.text}</div>
            <div style="font-size: 12px; color: #666;">${new Date(h.timestamp).toLocaleString()}</div>
          </div>
        `).join('')}
      </div>
    `;
    panel.style.display = 'block';
    
    // 关闭按钮
    document.getElementById('close-panel').addEventListener('click', function() {
      panel.style.display = 'none';
    });
  }
  
  // 监听来自 popup 的消息
  chrome.runtime.onMessage.addListener(function(request, sender, sendResponse) {
    switch (request.action) {
      case 'viewNotes':
        showNotePanel();
        break;
      case 'clearAll':
        clearAllHighlights();
        sendResponse({ success: true });
        break;
      case 'getCount':
        sendResponse({ count: highlights.length });
        break;
    }
  });
  
  // 清除所有高亮
  function clearAllHighlights() {
    document.querySelectorAll('.web-highlight').forEach(el => {
      const text = document.createTextNode(el.textContent);
      el.parentNode.replaceChild(text, el);
    });
    highlights = [];
    saveHighlights();
  }
  
  // 初始化
  init();
})();
```

---

### 第 6 步：添加样式

**content.css：**
```css
/* 注入到网页的样式 */

.web-highlight {
  padding: 2px 4px;
  border-radius: 3px;
  transition: all 0.2s;
}

.web-highlight:hover {
  filter: brightness(0.9);
}

.highlight-menu-item {
  padding: 10px 16px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
}

.highlight-menu-item:hover {
  background: #f0f0f0;
}

/* 笔记面板样式 */
#note-panel {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

#note-panel .note-item {
  border-left: 3px solid #4285f4;
}
```

---

### 第 7 步：后台脚本

**background.js：**
```javascript
// background.js - 后台服务脚本

// 监听扩展安装
chrome.runtime.onInstalled.addListener(function(details) {
  if (details.reason === 'install') {
    console.log('扩展已安装');
    // 可以在这里打开欢迎页面
  } else if (details.reason === 'update') {
    console.log('扩展已更新到版本', chrome.runtime.getManifest().version);
  }
});

// 监听快捷键
chrome.commands.onCommand.addListener(function(command) {
  if (command === 'toggle-highlight') {
    // 执行高亮操作
    chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
      chrome.tabs.sendMessage(tabs[0].id, { action: 'quickHighlight' });
    });
  }
});

// 右键菜单
chrome.contextMenus.create({
  id: 'highlight-selection',
  title: '高亮选中文本',
  contexts: ['selection']
});

chrome.contextMenus.onClicked.addListener(function(info, tab) {
  if (info.menuItemId === 'highlight-selection') {
    chrome.tabs.sendMessage(tab.id, { 
      action: 'highlightFromMenu',
      selectionText: info.selectionText
    });
  }
});
```

---

## 🧪 测试扩展

### 本地加载测试

1. 打开 Chrome，访问 `chrome://extensions/`
2. 开启右上角「开发者模式」
3. 点击「加载已解压的扩展程序」
4. 选择你的项目文件夹
5. 扩展出现在列表中，表示加载成功

### 测试清单

**功能测试：**
- [ ] 点击扩展图标，弹出窗口正常显示
- [ ] 颜色选择功能正常
- [ ] 选中网页文字，可以高亮
- [ ] 高亮颜色正确
- [ ] 点击高亮文字有反应
- [ ] 「查看笔记列表」功能正常
- [ ] 「清除所有高亮」功能正常
- [ ] 刷新页面后高亮仍然存在
- [ ] 高亮计数正确

**兼容性测试：**
- [ ] 在不同网站测试
- [ ] 测试动态加载的页面
- [ ] 测试中文和英文内容

---

## 🐛 常见问题

### Q1: 扩展加载失败

**可能原因：**
- manifest.json 格式错误
- 文件路径不正确
- 图标文件缺失

**解决方法：**
```
向 AI 提问：
我的 Chrome 扩展加载失败，错误信息是：
[粘贴错误信息]

请帮我检查 manifest.json 并修复问题。
```

---

### Q2: content script 没有执行

**检查步骤：**
1. 打开开发者工具（F12）
2. 查看 Console 是否有错误
3. 确认 manifest.json 中 content_scripts 配置正确

---

### Q3: 无法保存数据

**可能原因：**
- 没有在 manifest.json 中声明 storage 权限
- 存储空间超出限制

---

### Q4: 高亮后页面布局错乱

**解决方法：**
- 使用更安全的 DOM 操作方式
- 避免跨元素选择
- 使用 CSS 类而不是直接修改样式

---

## 🚀 发布扩展

### 准备发布

1. **准备素材：**
   - 128x128 图标
   - 宣传图片（440x280）
   - 截图（1280x800）

2. **打包扩展：**
   - 在 `chrome://extensions/` 点击「打包扩展程序」
   - 生成 .crx 文件

### 发布到 Chrome 商店

1. 访问 [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/developer/dashboard)
2. 支付 $5 一次性注册费
3. 点击「添加新项目」
4. 上传 .crx 文件
5. 填写商店信息：
   - 名称、描述
   - 截图、宣传图
   - 分类、语言
6. 提交审核
7. 等待审核（通常 1-3 天）

---

## 📚 进阶学习

### 扩展能力

Chrome 扩展还可以做更多：
- **标签页管理** - 批量操作标签页
- **网络请求** - 拦截、修改请求
- **书签管理** - 同步、整理书签
- **下载管理** - 控制下载行为
- **通知** - 显示桌面通知

### 推荐资源

- [Chrome 扩展官方文档](https://developer.chrome.com/docs/extensions/)
- [Chrome 扩展示例](https://github.com/GoogleChrome/chrome-extensions-samples)
- [Extension Workshop](https://extensionworkshop.com/)

---

## ✅ 学习检查

完成本项目后，你应该掌握了：

- [ ] Chrome 扩展的基本结构
- [ ] manifest.json 配置方法
- [ ] content script 的作用
- [ ] popup 和 background 的区别
- [ ] 扩展与网页的通信
- [ ] chrome.storage 数据存储
- [ ] 扩展的测试和发布

---

**💡 小贴士：** Chrome 扩展开发是进入浏览器世界的敲门砖。掌握后，你可以打造各种提升效率的神器！

---

## 📚 相关章节

- [项目实战目录](./README.md)
- [Cursor 使用指南](../../02-Cursor使用指南.md)
- [Web 应用部署](./08-Web部署.md)
