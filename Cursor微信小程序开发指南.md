# Cursor + 微信开发者工具协同开发指南

## 概述

本指南将帮助你在 Cursor 中高效开发微信小程序，同时利用微信开发者工具进行预览和调试。

## 🚀 方案一：混合开发模式（推荐）

### 1. 环境准备

#### 1.1 安装必要工具

```bash
# 1. 下载并安装微信开发者工具
# https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

# 2. 确保 Cursor 已安装
# https://cursor.sh/
```

#### 1.2 项目目录设置

```
your-miniprogram/
├── .vscode/                 # Cursor/VS Code 配置
│   ├── settings.json
│   ├── extensions.json
│   └── tasks.json
├── pages/                   # 小程序页面
├── components/              # 自定义组件
├── utils/                   # 工具函数
├── app.js                   # 应用入口
├── app.json                 # 应用配置
├── app.wxss                 # 全局样式
├── project.config.json      # 项目配置
└── README.md
```

### 2. Cursor 配置优化

#### 2.1 安装推荐插件

在 Cursor 中安装以下插件：

```json
// .vscode/extensions.json
{
  "recommendations": [
    "ms-ceintl.vscode-language-pack-zh-hans",
    "johnsoncodehk.vscode-typescript-vue-plugin",
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-json",
    "formulahendry.auto-rename-tag",
    "christian-kohler.path-intellisense",
    "streetsidesoftware.code-spell-checker"
  ]
}
```

#### 2.2 工作区设置

```json
// .vscode/settings.json
{
  "files.associations": {
    "*.wxml": "html",
    "*.wxss": "css",
    "*.wxs": "javascript"
  },
  "emmet.includeLanguages": {
    "wxml": "html"
  },
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  },
  "prettier.documentSelectors": ["**/*.wxml", "**/*.wxss"],
  "files.exclude": {
    "**/node_modules": true,
    "**/.git": true,
    "**/miniprogram_npm": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/miniprogram_npm": true
  }
}
```

#### 2.3 代码片段配置

```json
// .vscode/snippets.json
{
  "Page Template": {
    "prefix": "wxpage",
    "body": [
      "Page({",
      "  data: {",
      "    $1",
      "  },",
      "",
      "  onLoad(options) {",
      "    $2",
      "  },",
      "",
      "  onReady() {",
      "    $3",
      "  },",
      "",
      "  onShow() {",
      "    $4",
      "  },",
      "",
      "  onHide() {",
      "    $5",
      "  },",
      "",
      "  onUnload() {",
      "    $6",
      "  }",
      "});"
    ],
    "description": "微信小程序页面模板"
  },
  "Component Template": {
    "prefix": "wxcomponent",
    "body": [
      "Component({",
      "  properties: {",
      "    $1",
      "  },",
      "",
      "  data: {",
      "    $2",
      "  },",
      "",
      "  methods: {",
      "    $3",
      "  },",
      "",
      "  lifetimes: {",
      "    attached() {",
      "      $4",
      "    },",
      "",
      "    detached() {",
      "      $5",
      "    }",
      "  }",
      "});"
    ],
    "description": "微信小程序组件模板"
  }
}
```

### 3. 开发工作流程

#### 3.1 项目初始化

```bash
# 1. 在 Cursor 中创建项目
mkdir english-agent-miniprogram
cd english-agent-miniprogram

# 2. 初始化 git
git init
echo "node_modules/" > .gitignore
echo "miniprogram_npm/" >> .gitignore
echo ".DS_Store" >> .gitignore

# 3. 创建基本文件结构
mkdir pages components utils services
touch app.js app.json app.wxss project.config.json
```

#### 3.2 同步开发流程

1. **在 Cursor 中编写代码**

   - 利用智能提示和代码补全
   - 使用 Git 进行版本控制
   - 代码格式化和语法检查

2. **在微信开发者工具中预览**

   - 打开微信开发者工具
   - 导入项目目录
   - 实时预览和调试

3. **文件监听自动刷新**
   ```json
   // project.config.json 中启用自动刷新
   {
     "setting": {
       "compileHotReLoad": true
     }
   }
   ```

## 🔧 方案二：命令行工具集成

### 1. 微信开发者工具 CLI

```bash
# 安装微信开发者工具命令行
# Windows: 在微信开发者工具安装目录找到 cli.bat
# macOS: /Applications/wechatwebdevtools.app/Contents/MacOS/cli

# 添加到系统 PATH 或创建别名
alias wxdev="/Applications/wechatwebdevtools.app/Contents/MacOS/cli"
```

### 2. 常用命令

```bash
# 打开项目
wxdev open --project /path/to/your/project

# 预览项目
wxdev preview --project /path/to/your/project

# 上传代码
wxdev upload --project /path/to/your/project -v 1.0.0 -d "版本描述"

# 构建 npm
wxdev build-npm --project /path/to/your/project
```

### 3. Cursor 任务配置

```json
// .vscode/tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "打开微信开发者工具",
      "type": "shell",
      "command": "wxdev",
      "args": ["open", "--project", "${workspaceFolder}"],
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      }
    },
    {
      "label": "预览小程序",
      "type": "shell",
      "command": "wxdev",
      "args": ["preview", "--project", "${workspaceFolder}"],
      "group": "build"
    },
    {
      "label": "构建 npm 包",
      "type": "shell",
      "command": "wxdev",
      "args": ["build-npm", "--project", "${workspaceFolder}"],
      "group": "build"
    }
  ]
}
```

## 🔧 方案三：文件同步方案

### 1. 使用符号链接

```bash
# 创建符号链接，让微信开发者工具监听 Cursor 项目目录
ln -s /path/to/cursor/project /path/to/wechat/devtools/project
```

### 2. 使用文件监听工具

```bash
# 安装 nodemon 或类似工具
npm install -g nodemon

# 监听文件变化并执行命令
nodemon --watch ./ --ext js,json,wxml,wxss --exec "echo '文件已更新'"
```

## 🎯 最佳实践

### 1. 代码组织

```javascript
// utils/request.js - 网络请求封装
export const request = (url, options = {}) => {
  return new Promise((resolve, reject) => {
    wx.request({
      url: `${baseURL}${url}`,
      method: options.method || "GET",
      data: options.data || {},
      header: {
        "Content-Type": "application/json",
        ...options.header,
      },
      success: resolve,
      fail: reject,
    });
  });
};

// utils/storage.js - 本地存储封装
export const storage = {
  set: (key, value) => {
    try {
      wx.setStorageSync(key, value);
    } catch (error) {
      console.error("Storage set error:", error);
    }
  },

  get: (key, defaultValue = null) => {
    try {
      return wx.getStorageSync(key) || defaultValue;
    } catch (error) {
      console.error("Storage get error:", error);
      return defaultValue;
    }
  },
};
```

### 2. 组件化开发

```javascript
// components/voice-button/voice-button.js
Component({
  properties: {
    disabled: {
      type: Boolean,
      value: false,
    },
    size: {
      type: String,
      value: "normal", // normal, large, small
    },
  },

  data: {
    isRecording: false,
  },

  methods: {
    onTouchStart() {
      if (this.data.disabled) return;
      this.setData({ isRecording: true });
      this.triggerEvent("recordstart");
    },

    onTouchEnd() {
      this.setData({ isRecording: false });
      this.triggerEvent("recordend");
    },
  },
});
```

### 3. 调试技巧

#### 3.1 在 Cursor 中设置断点调试

```javascript
// 使用 console.log 进行调试
console.log("Debug info:", data);

// 使用 wx.showToast 显示调试信息
wx.showToast({
  title: `Debug: ${JSON.stringify(data)}`,
  icon: "none",
  duration: 3000,
});

// 使用条件编译进行调试
// #ifdef MP-WEIXIN
console.log("微信小程序环境");
// #endif
```

#### 3.2 网络请求调试

```javascript
// services/api.js
class ApiService {
  constructor() {
    this.baseURL = "https://your-api.com";
    this.debug = true; // 开发环境开启调试
  }

  request(url, options = {}) {
    if (this.debug) {
      console.log("API Request:", { url, options });
    }

    return new Promise((resolve, reject) => {
      wx.request({
        url: `${this.baseURL}${url}`,
        ...options,
        success: (res) => {
          if (this.debug) {
            console.log("API Response:", res);
          }
          resolve(res);
        },
        fail: reject,
      });
    });
  }
}
```

## 🔄 版本控制集成

### 1. Git 配置

```bash
# .gitignore
node_modules/
miniprogram_npm/
.DS_Store
*.log
.vscode/settings.json
project.private.config.json
```

### 2. 提交钩子

```json
// package.json
{
  "scripts": {
    "lint": "eslint --ext .js .",
    "format": "prettier --write .",
    "pre-commit": "npm run lint && npm run format"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run pre-commit"
    }
  }
}
```

## 🚀 自动化部署

### 1. GitHub Actions 配置

```yaml
# .github/workflows/deploy.yml
name: Deploy to WeChat MiniProgram

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "16"

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Upload to WeChat
        run: |
          # 使用微信开发者工具 CLI 上传
          wxdev upload --project ./ -v ${{ github.sha }} -d "Auto deploy from GitHub"
```

## 📱 真机调试

### 1. 预览二维码

```bash
# 生成预览二维码
wxdev preview --project /path/to/project --qr-format terminal
```

### 2. 远程调试

- 在微信开发者工具中开启远程调试
- 使用手机微信扫码进入调试模式
- 在 Cursor 中查看调试日志

## 🎨 UI 开发技巧

### 1. 样式组织

```css
/* app.wxss - 全局样式 */
@import "./styles/variables.wxss";
@import "./styles/mixins.wxss";
@import "./styles/base.wxss";

/* styles/variables.wxss - 变量定义 */
:root {
  --primary-color: #4a90e2;
  --secondary-color: #7a7e83;
  --background-color: #f8f9fa;
  --border-radius: 8rpx;
  --spacing-small: 16rpx;
  --spacing-medium: 32rpx;
  --spacing-large: 48rpx;
}
```

### 2. 响应式设计

```css
/* 使用 rpx 单位适配不同屏幕 */
.container {
  padding: 32rpx;
  max-width: 750rpx;
  margin: 0 auto;
}

/* 使用媒体查询 */
@media (max-width: 600px) {
  .container {
    padding: 16rpx;
  }
}
```

## 🔧 性能优化

### 1. 代码分包

```json
// app.json
{
  "pages": ["pages/index/index"],
  "subPackages": [
    {
      "root": "pages/chat",
      "pages": ["index/index", "history/history"]
    }
  ],
  "preloadRule": {
    "pages/index/index": {
      "network": "all",
      "packages": ["pages/chat"]
    }
  }
}
```

### 2. 图片优化

```javascript
// utils/image.js
export const compressImage = (src, quality = 0.8) => {
  return new Promise((resolve) => {
    wx.compressImage({
      src,
      quality,
      success: resolve,
    });
  });
};
```

## 📊 监控和分析

### 1. 性能监控

```javascript
// utils/monitor.js
export const performanceMonitor = {
  start: (name) => {
    console.time(name);
  },

  end: (name) => {
    console.timeEnd(name);
  },

  memory: () => {
    const performance = wx.getPerformance();
    console.log("Memory usage:", performance.memory);
  },
};
```

### 2. 错误收集

```javascript
// app.js
App({
  onError(error) {
    // 收集错误信息
    console.error("App Error:", error);

    // 上报错误（可选）
    wx.request({
      url: "https://your-api.com/error-report",
      method: "POST",
      data: {
        error: error.toString(),
        timestamp: Date.now(),
        userInfo: wx.getStorageSync("userInfo"),
      },
    });
  },
});
```

---

## 总结

通过以上配置，你可以在 Cursor 中享受现代化的代码编辑体验，同时利用微信开发者工具的强大调试功能。这种混合开发模式能够显著提高开发效率。

**推荐工作流程：**

1. 在 Cursor 中编写和管理代码
2. 使用 Git 进行版本控制
3. 在微信开发者工具中预览和调试
4. 使用命令行工具进行自动化操作

这样既能享受 Cursor 的 AI 辅助编程功能，又能充分利用微信开发者工具的专业调试能力。
