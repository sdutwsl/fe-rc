### 基本功能
webpack根据webpack.config.js中的入口文件，在入口文件里识别模块依赖，不管这里的模块依赖是用CommonJS写的，还是ES6 Module规范写的，webpack会自动进行分析，并通过转换、编译代码，打包成最终的文件。最终文件中的模块实现是基于webpack自己实现的webpack_require（es5代码），所以打包后的文件可以跑在浏览器上。

同时以上意味着在webapck环境下，你可以只使用ES6 模块语法书写代码（通常我们都是这么做的），也可以使用CommonJS模块语法，甚至可以两者混合使用。因为从webpack2开始，内置了对ES6、CommonJS、AMD 模块化语句的支持，webpack会对各种模块进行语法分析，并做转换编译。

### 简述原理
同步：
webpack之所以能处理不同风格的模块化方式，是因为webpack通过编译源码，处理入口文件及遍历它同步依赖的各个文件，将这些文件编译成一个个模块（函数），并缓存在 __webpack_modules__ 中；立即调用入口文件对应的模块（函数），在执行的过程中，通过 __webpack_require__ 来调用各个依赖的模块（函数）。总的来说webpack就是通过编译源码，实现了自己的模块化方式，统一处理了各个模块化的风格。
异步：
原理很简单，就是利用的 jsonp 的实现原理加载模块，只是在这里并不是从 server 拿数据而是从其他模块中。
整体的流程为：
1、加载入口 js 文件,__webpack_require__(__webpack_require__.s = 0)
2、执行入口 js 文件：modules[moduleId].call(module.exports, module, module.exports, webpack_require);
```
(function(module, exports, __webpack_require__) {
  eval(
    'module.exports = __webpack_require__(/*! D:\\webpack\\src\\index.js */"./src/index.js");\n\n\n//# sourceURL=webpack:///multi_./src/index.js?'
  );
  /***/
});

//和
eval(
  '\r\nconst css = __webpack_require__.e(/*! import() */ 0).then(__webpack_require__.t.bind(null, /*! ./index.css */ "./src/index.css", 7))\r\nconst css2 = __webpack_require__.e(/*! import() */ 1).then(__webpack_require__.t.bind(null, /*! ./index2.css */ "./src/index2.css", 7))\r\n\n\n//# sourceURL=webpack:///./src/index.js?'
);
```
3、由于上述代码分别__webpack_require__.e了0 和 1，分别使用类jsonp的方式异步加载对应 chunk，并缓存到 promise 的 resolve 中，并标记对应 chunk 已经加载**
4、调用对应 chunk 模块时会在 window 上注册一个 webpackJsonp 数组，window['webpackJsonp'] = window['webpackJsonp'] || []。并且执行push操作。由于push操作是使用webpackJsonpCallback进行重写的，所以每当执行push的时候就会触发webpackJsonpCallback. webpackJsonpCallback 标记对应 chunk 已经加载并执行代码。

### 随记
`optimization:{runtimeChunk: {name: 'runtime'}}`
这样会让webpack将runtime与模块注册代码分开打包
