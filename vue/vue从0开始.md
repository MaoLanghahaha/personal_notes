### 一、起步

安装npm
安装vue
安装vue-cli
安装git

### 二、項目初始化

#### 1、目录结构划分

主要是对src的划分

* public -- 此文件夹下的文件，打包后会原封不动（但会丑化压缩）的复制到dist文件夹下
* src
  * common -- 公共js文件
    * const.js -- 公共常量
    * utils.js -- 公共工具方法
    * minxin.js -- 混入
  * assets -- 静态资源文件夹
    * img -- 图片
    * css -- 样式
  * components -- 组件（公共）
    * common -- 业务无关的公共组件（其他项目可复用）
    * content -- 业务相关的公共组件（当前项目业务相关的公共组件，其他项目不可复用）
  * view -- 视图
  * router -- 路由
  * store -- 状态管理
  * network -- api
  * App.vue
  * main.js

#### 2、统一浏览器初始化css样式

  ##### *normalize.css[^normalize.css]*
  下载复制到src/assets/css下, 新建base.css（当前项目初始化样式配置）,base.css @import normalize.css, App.vue @impot base.css, main.js import App.vue (模块依赖打包需要)

  ##### *初始化base.css*
  统一变量
  ```css
  :root {
    --color-text: #666;
    --color-high-text: #ff5777; /* 文字高亮色 */
    --color-tint: #ff8198; /* 组件背景色 */
    --color-background: #fff;
    --font-size: 14px;
    --line-height: 1.5;
  }
  ```
  样式初始化
  ```css
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  body {
    height: 100vh;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Liberation Sans", "PingFang SC", "Microsoft YaHei", "Hiragino Sans GB", "Wenquanyi Micro Hei", "WenQuanYi Zen Hei", "ST Heiti", SimHei, SimSun, "WenQuanYi Zen Hei Sharp", sans-serif;
    font-size: var(--font-size);
    color: var(--color-text);
    background: var(--color-background);
    line-height: var(--line-height);
    user-select: none;
  }
  ```
#### 3、设置项目路径别名、定义项目编码规范

  ##### *创建vue.config.js*[^vue.config.js]文件
  ```javascript
  module.exports = {
    configureWebpack: { // 配置webpack
      resolve: { // 解决路径相关的一些问题
        // extensions:['.js', '.vue', '.json'], // 配置需要省略的文件后缀名，vue-cli3及以上默认的webpack配置（所以不需要再配置）
        alias: {
          // '@':'src' // vue-cli3及以上默认配置
          'assets':'@/assets', // vue-cli2写法：'assets':'src/assets'
          'common':'@/common',
          'components':'@/components',
          'network':'@/network',
          'views':'@/views',
        }
      }
    }
  }
  ```
  ```javacript
  //例子：
  App.vue
  import './assets/css/base.css' => import 'assets/css/base.css'
  ```
  ##### *创建.editorconfig*[^.editorConfig]文件
  ```.editorconfig
  # 告诉EditorConfig插件，这是根文件，不用继续往上查找
  root = true

  # 匹配全部文件
  [*]

  # 结尾换行符，可选"lf"、"cr"、"crlf"
  end_of_line = lf

  # 在文件结尾插入新行
  insert_final_newline = true

  # 删除一行中的前后空格
  trim_trailing_whitespace = true
  
  # 设置字符集
  charset = utf-8

  # 缩进风格，可选"space"、"tab"
  indent_style = space

  # 缩进的空格数
  indent_size = 2
  ```
  ##### *vsCode配置editorconfig*
  * 在当前项目根目录下添加.editorconfig文件
  * 安装EditorConfig扩展，插件市场搜索EditorConfig for vs code 安装
  * 全局安装或局部安装editorconfig依赖包(npm install -g editorconfig | npm install -D editorconfig)
  * 打开需要格式化的文件并手动格式化代码（Mac OS ：shift+option+f | Windows ：shift+alt+f）

  ##### [拓展 ESlint+Prettier+EditorConfig+VScode](https://juejin.cn/post/6844904138661330957)

### 三、项目模块划分
#### 框架组件创建
#### 框架路由搭建



### 将数据整合传递给组件
> 对于复杂的数据，将其整合成一个对象传递给组件，组件只需面向一个对象进行开发。
```javascript
// network/detail.js
export class GoodInfo { // 定义一个类
  constructor(itemInfo, columns, services) {
    this.title = itemInfo.title;
      this.desc = itemInfo.desc;
      this.newPrice = itemInfo.price;
      this.oldPrice = itemInfo.oldPrice;
      this.realPrice = itemInfo.lowNowPrice;
      this.discount = itemInfo.discountDesc;
      this.columns = columns;
      this.services = services;
  }
}
```


[^normalize.css]:A modern alternative to CSS resets
[^vue.config.js]:vue.config.js 是一个可选的配置文件，如果项目的 (和 package.json 同级的) 根目录中存在这个文件，那么它会被 @vue/cli-service 自动加载。
    ```javascript
    // vue.config.js

    /**
     * @type {import('@vue/cli-service').ProjectOptions}
     */
    module.exports = {
      // 选项...
    }
    ```
[^.editorConfig]:editorConfig
    * editorConfig帮助开发人员在不同的编辑器和IDE之间定义和维护一致的编码样式
    * 此文件用来定义项目的编码规范，编辑器的行为会与.editorconfig 文件中定义的一致，并且其优先级比编辑器自身的设置要高，这在多人合作开发项目时十分有用而且必要
    * 编辑器默认支持editorConfig，如webstorm；而有些编辑器则需要安装editorConfig插件，如ATOM、Sublime、VS Code等。当打开一个文件时，EditorConfig插件会在打开文件的目录和其每一级父目录查找.editorconfig文件，直到有一个配置文件root=true
    * EditorConfig的配置文件是从上往下读取的并且最近的EditorConfig配置文件会被最先读取.。匹配EditorConfig配置文件中的配置项会按照读取顺序被应用, 所以最近的配置文件中的配置项拥有优先权。如果.editorconfig文件没有进行某些配置，则使用编辑器默认的设置