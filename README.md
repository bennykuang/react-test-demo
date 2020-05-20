# 基于 Jest + Enzyme 的 React 单元测试

文章主要内容如下：

- 前置知识
- Jest 和 Enzyme 的基本介绍
- 测试环境搭建
- 测试脚本编写
  - UI 组件测试
  - Reducer 测试
- 运行并调试
- 参考资料

## 前置知识

### 为什么要进行测试

- 测试可以确保得到预期的结果
- 作为现有代码行为的描述
- 促使开发者写可测试的代码，一般可测试的代码可读性也会高一点
- 如果依赖的组件有修改，受影响的组件能在测试中发现错误

### 测试类型

- 单元测试：指的是以原件的单元为单位，对软件进行测试。单元可以是一个函数，也可以是一个模块或一个组件，基本特征就是只要输入不变，必定返回同样的输出。一个软件越容易些单元测试，就表明它的模块化结构越好，给模块之间的耦合越弱。React 的组件化和函数式编程，天生适合进行单元测试
- 功能测试：相当于是黑盒测试，测试者不了解程序的内部情况，不需要具备编程语言的专门知识，只知道程序的输入、输出和功能，从用户的角度针对软件界面、功能和外部结构进行测试，不考虑内部的逻辑
- 集成测试：在单元测试的基础上，将所有模块按照设计要求组装成子系统或者系统，进行测试
- 冒烟测试：在正式全面的测试之前，对主要功能进行的与测试，确认主要功能是否满足需要，软件是否能正常运行

### 开发模式

- TDD: 测试驱动开发，英文为 Testing Driven Development，强调的是一种开发方式，以测试来驱动整个项目，即先根据接口完成测试编写，然后在完成功能是要不断通过测试，最终目的是通过所有测试
- BDD: 行为驱动测试，英文为 Behavior Driven Development，强调的是写测试的风格，即测试要写的像自然语言，让项目的各个成员甚至产品都能看懂测试，甚至编写测试

TDD 和 BDD 有各自的使用场景，BDD 一般偏向于系统功能和业务逻辑的自动化测试设计；而 TDD 在快速开发并测试功能模块的过程中则更加高效，以快速完成开发为目的。

## Jest、Enzyme 介绍

Jest 是 Facebook 发布的一个开源的、基于 Jasmine 框架的 JavaScript 单元测试工具。提供了包括内置的测试环境 DOM API 支持、断言库、Mock 库等，还包含了 Spapshot Testing、 Instant Feedback 等特性。
Airbnb 开源的 React 测试类库 Enzyme 提供了一套简洁强大的 API，并通过 jQuery 风格的方式进行 DOM 处理，开发体验十分友好。不仅在开源社区有超高人气，同时也获得了 React 官方的推荐。

## 测试环境搭建

在开发 React 应用的基础上（默认你用的是 Webpack + Babel 来打包构建应用），你需要安装 Jest Enzyme，以及对应的 babel-jest

```
npm install jest enzyme babel-jest --save-dev
```

下载 npm 依赖包之后，你需要在 package.json 中新增属性，配置 Jest：

```JSON
  "jest": {
    "moduleFileExtensions": [
      "js",
      "jsx"
    ],
    "moduleNameMapper": {
      "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$": "<rootDir>/__mocks__/fileMock.js",
      ".*\\.(css|less|scss)$": "<rootDir>/__mocks__/styleMock.js"
    },
    "transform": {
      "^.+\\.js$": "babel-jest"
    }
  },
```

并新增 test scripts

```
"scripts": {
    "dev": "NODE_ENV=development webpack-dev-server  --inline --progress --colors --port 3000 --host 0.0.0.0 ",
    "test": "jest"
  }

```

其中 :

- moduleFileExtensions：代表支持加载的文件名，与 Webpack 中的 resolve.extensions 类似
- moduleNameMapper：代表需要被 Mock 的资源名称。如果需要 Mock 静态资源（如 less、scss 等），则需要配置 Mock 的路径 <rootDir>/**mocks**/yourMock.js
- transform 用于编译 ES6/ES7 语法，需配合 babel-jest 使用

上面三个是常用的配置，更多 Jest 配置见官方文档：Jest Configuration

## Jest

globals API

- describe(name, fn)：描述块，将一组功能相关的测试用例组合在一起
- it(name, fn, timeout)：别名 test，用来放测试用例
- afterAll(fn, timeout)：所有测试用例跑完以后执行的方法
- beforeAll(fn, timeout)：所有测试用例执行之前执行的方法
- afterEach(fn)：在每个测试用例执行完后执行的方法
- beforeEach(fn)：在每个测试用例执行之前需要执行的方法

全局和 describe 都可以有上面四个周期函数，describe 的 after 函数优先级要高于全局的 after 函数，describe 的 before 函数优先级要低于全局的 before 函数

## 测试脚本编写

在根目录下新建文件夹，命名："**test**"
在该文件夹下新建文件："example.test.js", 内容如下：

```
describe('test', () => {
  it('test', () => {
    expect('Hello World!').toBe('Hello World!');
  });
});

```
