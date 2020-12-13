# Micro Frontends Pattern Comparison

> Link: https://blog.bitsrc.io/microfrontend-pattern-comparison-c50a9d2e4172
> Author: Florian Rappl

## 介绍

简单了解了下什么是微前端：

> Link: https://medium.com/@bhargavshah2011/what-is-micro-frontends-mfes-e77af73414af
> Author: Bhargav Shah

The idea behind Micro Frontends is to think about a website or web app as a composition of features which are owned by independent teams. Each team has a distinct area of business or mission it cares about and specialises in.

**简单来说，微前端能够将很大的东西切成更小、更易于管理的部分，然后明确地表明它们之间的依赖性。技术选择、代码库、团队以及发布流程都能够彼此独立地操作和发展，无需过多的协调。**

文章提了四种不同的微前端集成方案：

## Build-Time Integration

各个团队可以独立开发，最终得到一个整体的可交付物。

- 优点

  - Type checking
  - Runtime optimization
  - Easy for migration

- 挑战：当某个微服务变化时，整个应用会 rebuild，当微服务数量很多时，会造成服务器压力。

  - Dynamic loading
  - Build times

- 使用场景：
  - Combination with server-side integration
  - 定义良好的小型外包程序
  - Use Webpack with the module federation plugin.

## Server-Side Integration

配合反向代理实现 SSR 集成构建微服务。

- 优点

  - Best performance
  - Dynamic loading
  - SEO 搜索引擎优化

- 挑战
  - Framework integration
  - Microfrontend isolation
  - Development environment

针对某些 content-heavy sites 的场景可以做一些尝试。

## Run-Time Integration via iframe

运行时利用 ifram 内嵌窗口，运行其他独立部署的前端内容。

- 优点

  - Strong isolation
  - Full flexibility
  - Web-native

- 挑战
  - No sharing possible
  - Difficult to sustain great UX
  - Worst performance

容易隔离会使 iframe 不够灵活，使路由、历史记录和深层链接变得更加复杂，并且很难做成响应式页面。

## Run-Time Integration via script

是目前比较受欢迎的微服务框架之一，类似于插件机制（ VS Code 用的），每个微前端都对应一个 `<script></script>` 标签，容器应用程序确定应该安装哪些微应用，并调用相关函数以告知微应用何时以及在何处进行渲染。

网上找到了一段比较直观易懂的代码：

```
<html>
  <head>
    <title>Feed me!</title>
  </head>
  <body>
    <h1>Welcome to Feed me!</h1>

    <!-- 这些脚本不会马上渲染应用 -->
    <!-- 而是分别暴露全局变量 -->
    <script src="https://browse.example.com/bundle.js"></script>
    <script src="https://order.example.com/bundle.js"></script>
    <script src="https://profile.example.com/bundle.js"></script>

    <div id="micro-frontend-root"></div>

    <script type="text/javascript">
      // 这些全局函数是上面脚本暴露的
      const microFrontendsByRoute = {
        '/': window.renderBrowseRestaurants,
        '/order-food': window.renderOrderFood,
        '/user-profile': window.renderUserProfile,
      };
      const renderFunction = microFrontendsByRoute[window.location.pathname];

      // 渲染第一个微应用
      renderFunction('micro-frontend-root');
    </script>
  </body>
</html>
```

- 优点

  - Very dynamic and flexible
  - Best developer experience

- 挑战
  - Weak isolation 弱隔离
  - Requires JavaScript

## 感想

初次接触微前端这个概念，单靠这篇文章肯定是不够的，网上也有大量相关文章，也说明微前端的流行。美团就采用了[基于 React 的中心路由基座式微前端](https://tech.meituan.com/2020/02/27/meituan-waimai-micro-frontends-practice.html)作了相关实践，通过陈宇宁同学的笔记也学到了许多额外的知识，后续会继续关注并学习相关知识。

Stay hungrey. Stay foolish.
