# 🐱 猫猫导航 (Mao Nav)

> 一个简洁美观的个人导航网站，支持分类管理和自定义收藏夹

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Vue](https://img.shields.io/badge/Vue-3.5.17-4FC08D?logo=vue.js)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-5.4.10-646CFF?logo=vite)](https://vitejs.dev/)
[![Cloudflare](https://img.shields.io/badge/Deploy-Cloudflare%20Pages-F38020?logo=cloudflare)](https://pages.cloudflare.com/)

## ✨ 特性

- 🎨 **现代化设计** - 简洁美观的界面，支持响应式布局
- 📱 **多设备适配** - 完美支持桌面端、平板和移动端
- 🔥 **分类管理** - 支持自定义分类和网站管理
- ⚡ **快速访问** - 基于 Vue 3 + Vite 构建，加载速度极快
- 🌐 **免费部署** - 支持 Cloudflare Pages 免费部署
- 💾 **数据存储** - 可选择使用 Cloudflare R2 存储数据
- 🛠️ **易于定制** - 简单的配置即可个性化你的导航

## 即将推出
  将增加管理员界面，支持可视化添加/编辑分类和网站


## 🚀 快速开始

### 🚀 一键部署到 Cloudflare（推荐）

**1. Fork 本项目**
- 点击页面右上角的 **"Fork"** 按钮
- 将项目 Fork 到你的 GitHub 账号下

**2. 在 Cloudflare Pages 控制台部署**
1. 访问 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 注册/登录 Cloudflare 账号（免费）
3. 点击左侧菜单 **"Workers & Pages"**
4. 点击 **"Create application"** → **"Pages"** → **"Connect to Git"**
5. 授权 GitHub 并选择你 Fork 的 `mao_nav` 仓库
6. 配置构建设置：
   - **Framework preset**: `Vue`
   - **Build command**: `npm run build`
   - **Build output directory**: `dist`

7. 点击 **"Save and Deploy"**

✅ **完成！** 几分钟后你就有了自己的导航网站，每次修改代码都会自动重新部署。

**3. 自定义你的导航**
- 编辑 `src/mock/mock_data.js` 文件，添加你自己的网站分类和链接
- 提交更改，Cloudflare 会自动重新部署

**4. 绑定自定义域名（可选）**
- 在 Cloudflare Pages 项目设置中点击 **"Custom domains"**
- 添加你的域名并按提示配置 DNS

#### 🗃️ 使用 R2 存储数据（高级功能，可选）

如果你想使用 Cloudflare R2 来存储导航数据而不是本地 `mock_data.js` 文件：

**1. 创建 R2 存储桶并上传数据**
```bash
# 安装 Wrangler CLI
npm install -g wrangler

# 登录 Cloudflare
wrangler login

# 创建存储桶
wrangler r2 bucket create navigation-data

# 将你的导航数据上传为 JSON 文件
# 创建 categories.json 文件，内容与 mock_data.js 中的数据结构相同
wrangler r2 object put navigation-data/categories.json --file=categories.json
```

**2. 启用 R2 存储桶的公开访问**
```bash
wrangler r2 bucket create navigation-data --public
```

**3. 修改代码**
编辑 `src/apis/useNavigation.js`，将注释的 R2 代码取消注释：
```javascript
// 替换这行：
categories.value = mockData.categories

// 为：
const response = await fetch('https://your-bucket-name.r2.dev/categories.json')
if (!response.ok) throw new Error('Failed to fetch from R2')
categories.value = await response.json()
```

**注意**：大多数用户直接使用本地 `mock_data.js` 就足够了，R2 存储适合需要动态更新数据或多人协作管理的场景。




### 本地开发

1. **克隆项目**
```bash
git clone https://github.com/your-username/mao_nav.git
cd mao_nav
```

2. **安装依赖**
```bash
npm install
```

3. **启动开发服务器**
```bash
npm run dev
```

4. **打开浏览器访问** `http://localhost:5173`

### 项目结构

```
mao_nav/
├── src/
│   ├── apis/           # API 接口
│   ├── assets/         # 静态资源（图片、样式等）
│   ├── components/     # Vue 组件
│   ├── mock/          # 模拟数据
│   ├── router/        # 路由配置
│   ├── stores/        # Pinia 状态管理
│   ├── views/         # 页面组件
│   ├── App.vue        # 根组件
│   └── main.js        # 入口文件
├── public/            # 公共静态文件
├── index.html         # HTML 模板
├── package.json       # 项目配置
├── vite.config.js     # Vite 配置
└── wrangler.toml      # Cloudflare 部署配置
```

## 🎯 自定义配置

### 修改导航数据

编辑 `src/mock/mock_data.js` 文件来自定义你的导航分类和网站：

```javascript
export const mockData = {
  categories: [
    {
      id: "my-favorites",
      name: "我的常用",
      icon: "💥",
      order: 0,
      sites: [
        {
          id: "example",
          name: "示例网站",
          url: "https://example.com",
          description: "网站描述",
          icon: "https://example.com/favicon.ico"
        }
      ]
    }
  ]
}
```

### 自定义样式

- 主要样式文件：`src/assets/main.css`
- 基础样式：`src/assets/base.css`


## 🛠️ 开发命令

```bash
# 开发模式
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview

# 代码检查和修复
npm run lint
```

## 📋 部署清单

在部署前请检查：

- [ ] 已修改 `src/mock/mock_data.js` 为你的个人数据
- [ ] 已更新 `package.json` 中的项目信息
- [ ] 已配置 Cloudflare 账号和 API Token
- [ ] 已测试构建命令 `npm run build`
- [ ] 已验证 `dist` 目录生成正常

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的修改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开一个 Pull Request

## 📄 许可证

本项目基于 MIT 许可证开源 - 查看 [LICENSE](LICENSE) 文件了解详情

## 🙏 致谢

- [Vue.js](https://vuejs.org/) - 渐进式 JavaScript 框架
- [Vite](https://vitejs.dev/) - 下一代前端构建工具
- [Cloudflare Pages](https://pages.cloudflare.com/) - 现代化的 JAMstack 平台
- [Pinia](https://pinia.vuejs.org/) - Vue.js 状态管理库

## 📞 联系方式

如果你有任何问题或建议，欢迎通过以下方式联系：

- 提交 [Issue](https://github.com/your-username/mao_nav/issues)
- 发起 [Discussion](https://github.com/your-username/mao_nav/discussions)

---

⭐ 如果这个项目对你有帮助，请给它一个 Star！
