### 概述
在 ES6 前， 实现模块化使用的是 RequireJS 或者 seaJS（分别是基于 AMD 规范的模块化库，  和基于 CMD 规范的模块化库）。
ES6 引入了模块化，其设计思想是在编译时就能确定模块的依赖关系，以及输入和输出的变量。
ES6 的模块化分为导出（export） @与导入（import）两个模块。
### 特点
ES6 的模块自动开启严格模式，不管你有没有在模块头部加上 use strict;。
每个模块都有自己的上下文，每一个模块内声明的变量都是局部变量，不会污染全局作用域。
每一个模块只加载一次（是单例的）， 若再去加载同目录下同文件，直接从内存中读取。
### export 与 import
基本用法
模块导入导出各种类型的变量，如字符串，数值，函数，类。不论值还是引用。
export 命令可以出现在模块的任何位置，但必需处于模块顶层。
import 命令会提升到整个模块的头部，首先执行。
import导入的变量是为只读，不允许再次赋值。


### as
as可以重命名导入或者导出变量

### default
在一个文件或模块中，export、import可以有多个，export default仅有一个

### *
`import * as x from 'xx.js' 就是导入所有导出 需要配合as使用 默认导出会被放在default字段

### import()
返回一个promise以支持动态导入
```
    import('/modules/my-module.js')
      .then(module => {
        module.loadPageInto(main);
      })
      .catch(err => {
        main.textContent = err.message;
      });
```
### 与require（CommonJS/AMD/UMD/CMD）的区别
1、遵循的模块化规范不一样
2、出现的时间不同
3、使用形式不一样
4、本质区别：
（1）、CommonJS 还是 ES6 Module 输出都可以看成是一个具备多个属性或者方法的对象；
（2）、default 是 ES6 Module 所独有的关键字，export default fs 输出默认的接口对象，import fs from 'fs' 可直接导入这个对象
（3）、ES6 Module 中导入模块的属性或者方法是强绑定的，包括基础类型；而 CommonJS 则是普通的值传递或者引用传递。