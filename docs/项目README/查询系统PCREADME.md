# 贵煤查询系统---PC

[TOC]

## 1. 技术栈

- [Vue-CLI](https://cli.vuejs.org/guide/cli-service.html#using-the-binary)
- [Vue](https://cn.vuejs.org/v2/guide/)
- [Vue-Router](https://router.vuejs.org/)
- [Vuex](https://vuex.vuejs.org/)
- [Element](http://element.eleme.io/#/zh-CN/component/installation)
- [Postcss](https://github.com/postcss/postcss)
- [Jest]()
- [Eslint](https://eslint.org/)




##  2. 项目结构

```
├── public                     // 公用文件,index 及 favicon
├── config                     // 配置相关
├── src                        // 源代码
│   ├── api                    // 所有请求
│   ├── assets                 // 主题 字体 图片等静态资源
│   ├── components             // 全局公用组件
│   ├── directive              // 全局指令
│   ├── router                 // 路由
│   ├── store                  // 全局 store管理
│   ├── styles                 // 全局样式
│   ├── utils                  // 全局公用方法
│   ├── views                  // view
│   ├── App.vue                // 入口页面
│   ├── main.js                // 入口 加载组件 初始化等
│   └── permission.js          // 权限管理
├── babel.config.js            // babel-loader 配置
├── .eslintrc.js               // eslint 配置项
├── .gitignore                 // git 忽略项
└── package.json               // package.json
└── jest.config.js             // jest配置文件
└── .postcssrc.js              // postcss配置文件
└── Vue.config.js              // vue-service配置文件
```



## 3. 项目命令

- 下载依赖

```
npm install
```

- 启动项目

```
npm run dev
```

- 生成环境打包

```
npm run build
```

- 检查代码格式

```
npm run lint
```

- 单元测试

```
npm run test:unit
```



## 项目使用

- 图片文件引用

```
# HTML 及 JavaScript代码
使用@
@ = src
例: 
@/assets/images/xx.png

# css
使用resolve() 或者 inline(base64, 仅限小图片)
如: 
background: resolve('logo.png');
background: inline('logo.png');

```

- css别名和变量

```
文件位置: src/styles/variables.scss
创建新别名请尽量保证易于理解.
```



