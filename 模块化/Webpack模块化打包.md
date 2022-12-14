### 基本功能
webpack根据webpack.config.js中的入口文件，在入口文件里识别模块依赖，不管这里的模块依赖是用CommonJS写的，还是ES6 Module规范写的，webpack会自动进行分析，并通过转换、编译代码，打包成最终的文件。最终文件中的模块实现是基于webpack自己实现的webpack_require（es5代码），所以打包后的文件可以跑在浏览器上。

同时以上意味着在webapck环境下，你可以只使用ES6 模块语法书写代码（通常我们都是这么做的），也可以使用CommonJS模块语法，甚至可以两者混合使用。因为从webpack2开始，内置了对ES6、CommonJS、AMD 模块化语句的支持，webpack会对各种模块进行语法分析，并做转换编译。

