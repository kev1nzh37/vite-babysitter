---
title: 1.介绍
order: 1
group:
  title: NPM 依赖解析和预构建
  order: 2

---

# NPM 依赖解析和预构建

## 介绍

本篇介绍构建类工具Vite的功能：依赖解析和预构建。

下面章节会结合Vite官方文档讲解。

### Vite的实现

1. **CommonJS 和 UMD 兼容性**: 开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。

2. **性能**： Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

3. **自动依赖搜寻**: 如果没有找到相应的缓存，Vite 将抓取你的源码，并自动寻找引入的依赖项（即 "bare import"，表示期望从 node_modules 解析），并将这些依赖项作为预构建包的入口点。预构建通过 esbuild 执行，所以它通常非常快。

4. **Monorepo 和链接依赖**: 在一个 monorepo 启动中，该仓库中的某个依赖可能会成为另一个包的依赖。Vite 会自动侦测没有从 node_modules 解析的依赖项，并将链接的依赖视为源码。它不会尝试打包被链接的依赖，而是会分析被链接依赖的依赖列表。

5. **缓存**: 文件系统缓存和浏览器缓存。

以上五个功能来自官方文档，我们会在接下来的章节中了解实现的原理。