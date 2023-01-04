# Nocalhost Docs & Website

> 由于使用 @docusaurus 构建特别复杂，需要安装繁多的 Js 开发组件，经常有安装连接失败，以及组件升级导致各种兼容问题，很难调试，
> 最后改用快速高效 mkdocs

该存储库包含 Nocalhost 网站和所有文档的源代码。它由[Docusaurus 2](https://docusaurus.io/)建造.

## 预习

克隆存储库到您当地的工作站。

### 安装

```bash
npm install
```

### 本地开发

```bash
npm run start
```

此命令启动本地开发服务器并打开浏览器窗口。大多数更改无需重新启动服务器即可反映。

### 构建

```bash
npm run build
```

此命令将静态内容生成构建目录，并可以使用任何静态内容托管服务提供服务。

## 添加或更新文档

当你添加或修改文档时，这三个文件(`docs`, `static` and `sidebars.js`)应该被考虑在内。

1. `docs/`, 主要的英文文档文件主要位于这个文件夹中。所有 markdown 文件需要遵循的格式，在开始的标题应该是以下格式:

```yaml
---
title: Title Name
---
```

2. `static`, 包含所有静态文件，如图像，样式表和字体。我们使用 mdx 格式的图片资源，请阅读【docusaurus 官方文档】(https://docusaurus.io/docs/markdown-features)了解降价功能。

```md
<figure className="img-frame">
  <img className="gif-img" src={useBaseUrl('/img/installation/vscode-market.png')} />
  <figcaption>VS Code Extension Market</figcaption>
</figure>
```

3. `sidebars.js`, 该文件包含 KubeVela 网站的导航信息。请阅读[docusaurus 的官方文档](https://docusaurus.io/docs/sidebar)，学习如何编写`sidebar.js`。

```json
    {
      type: 'category',
      label: 'Getting Started',
      collapsed: false,
      items: [
        'quick-start',
        'installation',
      ]
    },
```
