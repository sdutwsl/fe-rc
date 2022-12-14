### CommonJS规范

#### 基本规则
1、Node应用由模块组成，每个文件就是一个模块，模块内为私有作用域，即模块内的变量、函数、类都是私有的，对其他模块不可见。
2、若想定义跨模块全局变量，则应该使用`global`对象，此写法不被推荐。
3、于每个模块的内部，`module`变量代表当前模块，其`exports`属性（`module.exports`）是对外接口，加载一个模块就是加载该模块的exports属性。
4、`require`方法用于加载一个模块、模块加载的实质就是注入`exports`、`require`、`module`三个变量，然后执行模块的源码，并将`exports`的值输出。
5、CommonJS规范有如下特点：所有代码运行在模块作用域，不会污染全局作用域、模块可以多次被加载，但只会在第一次加载时运行一次，结果会被缓存、模块按照其在代码中出现的顺序被加载。

#### 核心对象`module`
```
{
    id          :string     //识别符，通常是带有绝对路径的模块文件名
    filename    :string     //模块的文件名，带有绝对路径
    loaded      :boolean    //表示模块是否已经完成加载
    parent      :object     //表示调用该模块的模块 入口模块为null
    children    :array      //表示该模块要用到的其他模块。
    exports     :object     //表示模块对外输出的值
    isPreloading:boolean    //模块是否在Node预加载阶段运行
    path        :string     //表示模块的目录名称
    paths       :array      //模块的搜索路径
}
```
#### 核心对象`exports`
1、它就是`module.exports`的引用
2、不要去赋值改变它
3、建议只使用module.exports
4、赋值给exports值类型的时候，仅仅发生了一次赋值。
#### 核心命令`require`
它其实不是一个全局命令，而是指向了`module.require`，后者调用了`Module._load`内部方法。
路径分析
文件**定位**
编译**执行**
1、`require`命令的基本功能是，读入并**执行**一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。
2、当 Node 遇到 require(X) 时，按下面的顺序处理：
（1）如果 X 是内置模块（比如 require('http'）)则返回该模块或者不再继续执行。
（2）如果 X 以 "./" 或者 "/" 或者 "../" 开头:
根据 X 所在的父模块，确定 X 的绝对路径。
将 X 当成文件，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
`X   X.js    X.json  X.node`
将 X 当成目录，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
`   X/package.json（main字段）         X/index.js         X/index.json         X/index.node`
（3）如果 X 不带路径
根据 X 所在的父模块，确定 X 可能的安装目录。依次在每个目录中，将 X 当成文件名或目录名加载。
（4）抛出 "not found"
3、删除缓存 `delete require.cache[moduleName]`
4、循环加载时，后加载的模块将会加载先加载的模块的不完整版本（即使是循环加载，调用也会有先后顺序）。
5、require.main可以用来判断模块是直接执行、还是被调用。`require.main === module`
6、内部流程
```
Module._load
Module.prototype._compile
```
#### 自己的思考
require就是按照顺序执行代码并返回exports而已，若缓存中存在，则直接返回缓存中的内容，而且是在执行代码之前，就将其加入了缓存中，这也是为什么循环加载会返回仅加载一半的模块。向module.exports.obj赋值的obj对象会被保留，向module.export.v赋值的v值会被拷贝，而其他的变量则会被隐藏（甚至回收？因为除了此module，没有其他的module可以用到了）


