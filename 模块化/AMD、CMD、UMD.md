### AMD与CMD
写法类似 重点在于区别
两个似乎都没什么人用了

### 两个函数
`define(id?: String, dependencies?: String[], factory: Function|Object);`
id 是模块的名字，它是可选的参数。
dependencies 指定了所要依赖的模块列表，它是一个数组，也是可选的参数，每个依赖的模块的输出将作为参数依次传入 factory 中。如果没有指定 dependencies，那么它的默认值是 ["require", "exports", "module"]。
`define(function(require, exports, module) {}）`
factory 是最后一个参数，它包裹了模块的具体实现，它是一个函数或者对象。如果是函数，那么它的返回值就是模块的输出接口或值。
`factory()`
`require([module], callback);`

### AMD实现库 `RequireJS`
异步加载： 使用 RequireJS，会在相关的 js 加载后执行回调函数，这个过程是异步的，所以它不会阻塞页面。
按需加载： 通过 RequireJS，你可以在需要加载 js 逻辑的时候再加载对应的 js 模块，不需要的模块就不加载，这样避免了在初始化网页的时候发生大量的请求和数据传输。
### CMD实现库 `Sea.js`（国内）
简单友好的模块定义规范：Sea.js 遵循 CMD 规范，可以像 Node.js 一般书写模块代码。
自然直观的代码组织方式：依赖的自动加载、配置的简洁清晰，可以让我们更多地享受编码的乐趣。

### 区别
对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
CMD 推崇依赖就近，AMD 推崇依赖前置。
AMD 的 API 默认是一个当多个用，CMD 的 API 严格区分，推崇职责单一。比如 AMD 里，require 分全局 require 和局部 require，都叫 require。CMD 里，没有全局 require，而是根据模块系统的完备性，提供 seajs.use 来实现模块系统的加载启动。CMD 里，每个 API 都简单纯粹。
还有一些细节差异，具体看这个规范的定义就好，就不多说了。

AMD | 速度快 | 会浪费资源 | 预先加载所有的依赖，直到使用的时候才执行
CMD | 只有真正需要才加载依赖 | 性能较差 | 直到使用的时候才定义依赖

### UMD
UMD (Universal Module Definition), 希望提供一个前后端跨平台的解决方案(支持AMD与CommonJS模块方式),。

实现原理
UMD的实现很简单：
先判断是否支持Node.js模块格式（exports是否存在），存在则使用Node.js模块格式。
再判断是否支持AMD（define是否存在），存在则使用AMD方式加载模块。
前两个都不存在，则将模块公开到全局（window或global）。