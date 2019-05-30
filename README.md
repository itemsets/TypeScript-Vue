# TypeScript-Vue

TypeScript结合Vue是否能擦出什么闪亮的火花~~

TypeScript 的设计目的应该是解决 JavaScript 的“痛点”：弱类型和没有命名空间，导致很难模块化，不适合开发大型程序。另外它还提供了一些语法糖来帮助大家更方便地实践面向对象的编程。

TypeScript 带来了可选的静态类型检查以及最新的 ECMAScript 特性。

一、基础类型 接口 泛型 模块 装饰器 声明文件  
二、结合Vue开发  
三、设计模式  
四、项目迁移  

## TypeScript 结合 Vue 是否友好

打包或优化甚至其它方面是否得到进展  
包括结合其它库或者vue官网设计是否合理  
哪些缺陷和哪些看点  

## VSCode warning

如果ts文件有报错：  

```text
[ts] Experimental support for decorators is a feature that is subject to change in a future release. Set the 'experimentalDecorators' option to remove this warning."
```

是有关装饰器得问题，读取不了。

在`tsconfig.json`修改或添加：  

```json
{  
    "compilerOptions":{  
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true,
    }
}
```

如果这样设置还有报错得话，那就是你的项目不是在VSCode窗口的根目录打来的。  
一个项目一个VSCode窗口，才有效(当前窗口下要有tsconfig.json)。

