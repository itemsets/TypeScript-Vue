## TypeScript 数据类型

- 原始数据类型
- 任意值
- 类型推论
- 联合类型
- 对象的类型——接口
- 数组的类型
- 函数的类型
- 类型断言
- 声明文件
- 内置对象
- 类型别名
- 字符串字面量类型
- 元组
- 枚举

### 原始数据类型

JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。
原始数据类型包括：number、string、boolean、null、undefined 以及 ES6 中的新类型 Symbol。

``` js
let  decLiteral:  number  =  5;
let  myName:  string  =  'ABCD';
let  isDone:  boolean  =  true;
  
// 空值
// void 表示没有任何返回值的函数。
function  alertName():  void {
alert('My name is Xanda');
}

// 声明一个 void 类型的变量没有什么用，
// 因为你只能将它赋值为 undefined 和 null：
let  unusable:  void  =  undefined;

// 在 TypeScript 中，可以使用 null 和 undefined 来定义这两个原始数据类型：
let  u:  undefined  =  undefined;
let  n:  null  =  null;
```

### 任意值

任意值（Any）用来表示允许赋值为任意类型。

```js
let  myFavoriteNumber:  any  =  'five';
myFavoriteNumber  =  5;
```
未指定其类型，那么它会被识别为任意值类型。

```js
let  something; =>  let  something:  any;
```

### 类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

```js
let  myFavoriteNumber  =  'five'; =>  let  myFavoriteNumber:  string  =  'five';
```

### 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种。

```js
let  myFavoriteNumber:  string  |  number;
myFavoriteNumber  =  'five';
myFavoriteNumber  =  5;
```

### 对象的类型——接口

在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implements）。
TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

理解为：一种对数据类型的初始化`限制`和`管理`。

```js
// 打个比方，假如我们定义一个Number类型,
// interface Number;
// let age: Number = 10;
// 接口定义
// 属性也是不允许多，不允许少

interface  Person {
	name:  string;
	age:  number;
}
let  xanda:Person  = {
	name:  'Xanda',
	age:  10
};

// 可选属性
interface  Person {
	name:  string;
	age?:  number;
}
let  xanda:Person  = {
	name:  'Xanda'
};

// 任意属性
// 需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集

interface  Person {
	name:  string;
	age?:  number;
	[propName:  string]:  any;
}
let  xanda:  Person  = {
	name:  'Xanda',
	gender:  'male'
};

// 只读属性
interface  Person {
	readonly  id:  number;
}
let  xanda:Person  = {
	id:  89757
};
xanda.id  =  9527;

// 使用 readonly 定义的属性 id 初始化后，又被赋值了，所以报错了。
// 注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候
```

### 数组的类型

  
```js
let  fibonacci:  number[] = [1, 1, 2, 3, 5];
let  fibonacci:  Array<number> = [1, 1, 2, 3, 5];
let  fibonacci:  (number  |  string)[] = [1, '1', 2, 3, 5]; // 联合类型和数组的结合
let  list:  any[] = ['Xanda Zhan', 10, { website:  'https://hsbc.com' }];
```

### 函数的类型

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：

```js
// 常数不能多或少
function  sum(x:  number, y:  number):  number {
	return  x  +  y;
}
sum(1, 2);
```

给函数定义类型：

```js
let  mySum: (x:  number, y:  number) =>  number  =  function (x:  number, y:  number):  number {
	return  x  +  y;
};
```

注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。
在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
在 ES6 中，=> 叫做箭头函数。

用接口定义函数的形状
```js
// 我们也可以使用接口的方式来定义一个函数需要符合的形状：
interface  SearchFunc {
	(source:  string, subString:  string):  boolean;
}
let  mySearch:  SearchFunc  =  function(source:  string, subString:  string) {
	return  source.search(subString)  !==  -1;
}
```

### 函数类型的重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

比如，我们需要实现一个函数 reverse，输入数字 123 的时候，输出反转的数字 321，输入字符串 'hello' 的时候，输出反转的字符串 'olleh'。

利用联合类型，我们可以这么实现：

 
```js
function  reverse(x:  number  |  string):  number  |  string {
	if  (typeof  x  ===  'number') {
		return  Number(x.toString().split('').reverse().join(''));
	} else  if  (typeof  x  ===  'string') {
		return  x.split('').reverse().join('');
	}
}
```

然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。
这时，我们可以使用重载定义多个 reverse 的函数类型：

```js
function  reverse(x:  number):  number;
function  reverse(x:  string):  string;
function  reverse(x:  number  |  string):  number  |  string {
	if  (typeof  x  ===  'number') {
		return  Number(x.toString().split('').reverse().join(''));
	} else  if  (typeof  x  ===  'string') {
		return  x.split('').reverse().join('');
	}
}
```

上例中，我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
  
### 类型断言

类型断言（Type Assertion）可以用来手动指定一个值的类型, 类型断言不是类型转换。
语法 <类型>值 或 值 as 类型

```js
2222.length  // Uncaught SyntaxError: Invalid or unexpected token
function  getLength(something:  string  |  number):  number {
	if  ((<string>something).length) {
		return  (<string>something).length;
	} else {
		return  something.toString().length;
	}
}
```

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

https://ts.xcatliu.com/basics/declaration-files

### 内置对象

JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

https://ts.xcatliu.com/basics/built-in-objects

### 类型别名

类型别名用来给一个类型起个新名字。

```js
type  Name  =  string;
type  NameResolver  = () =>  string;
type  NameOrResolver  =  Name  |  NameResolver;
function  getName(n:  NameOrResolver):  Name {
	if  (typeof  n  ===  'string') {
		return  n;
	} else {
		return  n();
	}
}
```

### 字符串字面量类型
  
字符串字面量类型用来约束取值只能是某几个字符串中的一个。

```js
type  EventNames  =  'click'  |  'scroll'  |  'mousemove';
function  handleEvent(ele:  Element, event:  EventNames) {
	// do something
}
```
类型别名与字符串字面量类型都是使用 type 进行定义。

### 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。
元组起源于函数编程语言（如 F#）,在这些语言中频繁使用元组。

```js
let  xanda: [string, number] = ['Xanda Zhan', 10];
```

### 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

```js
enum  Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```


## 类与接口

接口（Interfaces）可以用于对「对象的形状（Shape）」进行描述。

这一章主要介绍接口的另一个用途，对类的一部分行为进行抽象。

类实现接口
实现（implements）是面向对象中的一个重要概念。一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 implements 关键字来实现。这个特性大大提高了面向对象的灵活性。

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它：

```js
interface Alarm {
    alert();
}
class Door {
}
class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}
class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}
```

一个类可以实现多个接口：

```js
interface Alarm {
    alert();
}
interface Light {
    lightOn();
    lightOff();
}
class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

上例中，Car 实现了 Alarm 和 Light 接口，既能报警，也能开关车灯。

接口继承接口
接口与接口之间可以是继承关系：

```js
interface Alarm {
    alert();
}
interface LightableAlarm extends Alarm {
    lightOn();
    lightOff();
}
```

上例中，我们使用 extends 使 LightableAlarm 继承 Alarm。

接口继承类
接口也可以继承类：

```js
class Point {
    x: number;
    y: number;
}
interface Point3d extends Point {
    z: number;
}
let point3d: Point3d = {x: 1, y: 2, z: 3};
```

混合类型
​之前学习过，可以使用接口的方式来定义一个函数需要符合的形状：

```js
interface SearchFunc {
    (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

有时候，一个函数还可以有自己的属性和方法：

```js
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}
let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## 泛型

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。
简单的例子
首先，我们来实现一个函数 createArray，它可以创建一个指定长度的数组，同时将每一项都填充一个默认值：
```js
function createArray(length: number, value: any): Array<any> {
    let result = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

上例中，我们使用了之前提到过的数组泛型来定义返回值的类型。
这段代码编译不会报错，但是一个显而易见的缺陷是，它并没有准确的定义返回值的类型：
Array<any> 允许数组的每一项都为任意类型。但是我们预期的是，数组中每一项都应该是输入的 value 的类型。
这时候，泛型就派上用场了：
```js
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

上例中，我们在函数名后添加了 <T>，其中 T 用来指代任意输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用了。
接着在调用的时候，可以指定它具体的类型为 string。当然，也可以不手动指定，而让类型推论自动推算出来：
```js
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

多个类型参数
定义泛型的时候，可以一次定义多个类型参数：

```js
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

上例中，我们定义了一个 swap 函数，用来交换输入的元组。
泛型约束
在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法：
```js
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);
    return arg;
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'.
```
上例中，泛型 T 不一定包含属性 length，所以编译的时候报错了。
这时，我们可以对泛型进行约束，只允许这个函数传入那些包含 length 属性的变量。这就是泛型约束：
```js
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```
上例中，我们使用了 extends 约束了泛型 T 必须符合接口 Lengthwise 的形状，也就是必须包含 length 属性。
此时如果调用 loggingIdentity 的时候，传入的 arg 不包含 length，那么在编译阶段就会报错了：
```js
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

loggingIdentity(7);

// index.ts(10,17): error TS2345: Argument of type '7' is not assignable to parameter of type 'Lengthwise'.
```
多个类型参数之间也可以互相约束：
```js
function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = (<T>source)[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
```
上例中，我们使用了两个类型参数，其中要求 T 继承 U，这样就保证了 U 上不会出现 T 中不存在的字段。
泛型接口
之前学习过，可以使用接口的方式来定义一个函数需要符合的形状：
```js
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
当然也可以使用含有泛型的接口来定义函数的形状：
interface CreateArrayFunc {
    <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
进一步，我们可以把泛型参数提前到接口名上：
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

注意，此时在使用泛型接口的时候，需要定义泛型的类型。
泛型类
与泛型接口类似，泛型也可以用于类的类型定义中：

```js
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```
泛型参数的默认类型
在 TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。
```js
function createArray<T = string>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

## 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型：

函数的合并
​之前学习过，我们可以使用重载定义多个函数类型：
```js
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```
接口的合并
接口中的属性在合并时会简单的合并到一个接口中：
```js
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```
相当于：
```js
interface Alarm {
    price: number;
    weight: number;
}
```
注意，合并的属性的类型必须是唯一的：
```js
interface Alarm {
    price: number;
}
interface Alarm {
    price: number;  // 虽然重复了，但是类型都是 `number`，所以不会报错
    weight: number;
}
interface Alarm {
    price: number;
}
interface Alarm {
    price: string;  // 类型不一致，会报错
    weight: number;
}
​
// index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.
```
接口中方法的合并，与函数的合并一样：
```js
interface Alarm {
    price: number;
    alert(s: string): string;
}
interface Alarm {
    weight: number;
    alert(s: string, n: number): string;
}
```
相当于：
```js
interface Alarm {
    price: number;
    weight: number;
    alert(s: string): string;
    alert(s: string, n: number): string;
}
```
类的合并
类的合并与接口的合并规则一致。

## typescript + vue

vue-class-component
[vue-class-component](https://github.com/vuejs/vue-class-component) 对 Vue 组件进行了一层封装，让 Vue 组件语法在结合了 TypeScript 语法之后更加扁平化：

```js
<template>
  <div>
    <input v-model="msg">
    <p>msg: {{ msg }}</p>
    <p>computed msg: {{ computedMsg }}</p>
    <button @click="greet">Greet</button>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue'
  import Component from 'vue-class-component'

  @Component
  export default class App extends Vue {
    // 初始化数据
    msg = 123

    // 声明周期钩子
    mounted () {
      this.greet()
    }

    // 计算属性
    get computedMsg () {
      return 'computed ' + this.msg
    }

    // 方法
    greet () {
      alert('greeting: ' + this.msg)
    }
  }
</script>

```
上面的代码跟下面的代码作用是一样的
```js
export default {
  data () {
    return {
      msg: 123
    }
  }

  // 声明周期钩子
  mounted () {
    this.greet()
  }

  // 计算属性
  computed: {
    computedMsg () {
      return 'computed ' + this.msg
    }
  }

  // 方法
  methods: {
    greet () {
      alert('greeting: ' + this.msg)
    }
  }
}
```

vue-property-decorator
[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) 是在 vue-class-component 上增强了更多的结合 Vue 特性的装饰器，新增了这 7 个装饰器：

- @Emit
- @Inject
- @Model
- @Prop
- @Provide
- @Watch
- @Component (从 vue-class-component 继承)
- 
在这里列举几个常用的@Prop/@Watch/@Component, 更多信息，详见官方文档

```js

import { Component, Emit, Inject, Model, Prop, Provide, Vue, Watch } from 'vue-property-decorator'

@Component
export class MyComponent extends Vue {
  
  @Prop()
  propA: number = 1

  @Prop({ default: 'default value' })
  propB: string

  @Prop([String, Boolean])
  propC: string | boolean

  @Prop({ type: null })
  propD: any

  @Watch('child')
  onChildChanged(val: string, oldVal: string) { }
}
```
上面的代码相当于：

```js
export default {
  props: {
    checked: Boolean,
    propA: Number,
    propB: {
      type: String,
      default: 'default value'
    },
    propC: [String, Boolean],
    propD: { type: null }
  }
  methods: {
    onChildChanged(val, oldVal) { }
  },
  watch: {
    'child': {
      handler: 'onChildChanged',
      immediate: false,
      deep: false
    }
  }
}
```

## 其它

引入部分第三方库的时候需要额外声明文件
比如说我想引入vue-lazyload,虽然已经在本地安装，但是typescript还是提示找不到模块。原因是typescript是从node_modules/@types目录下去找模块声明，有些库并没有提供typescript的声明文件，所以就需要自己去添加

解决办法：在src/typings目前下建一个tools.d.ts文件，声明这个模块即可
```js
declare module 'vue-awesome-swiper' {
  export const swiper: any
  export const swiperSlide: any
}
 
declare module 'vue-lazyload'
```
对vuex的支持不是很好
在TypeScript里面使用不了mapState、mapGetters等方法，只能一个变量一个变量的去引用，这个要麻烦不少。不过使用vuex-class库之后，写法上也还算简洁美观
```js
export default class modules extends Vue {
  @State login: boolean; // 对应this.$store.state.login
  @State headline: StoreState.headline[]; // 对应this.$store.state.headline
 
  private swiperOption: Object = {
    autoplay: true,
    loop: true,
    direction: "vertical"
  };
 
  logoClick(): void {
    alert("点我干嘛");
  }
}
```

Vuex-Class
vuex-class是基于基于vue-class-component对Vuex提供的装饰器。它的作者同时也是vue-class-component的主要贡献者，质量还是有保证的。
```js
import Vue from "vue";
import Component from "vue-class-component";
import { State, Action, Getter } from "vuex-class";
 
@Component
export default class App extends Vue {
  name:string = 'Simon Zhang'
  @State login: boolean;
  @Action initAjax: () => void;
  @Getter load: boolean;
 
  get isLogin(): boolean {
    return this.login;
  }
 
  mounted() {
    this.initAjax();
  }
}
```
上面的代码就相当于：
```js
export default {
  data() {
    return {
      name: 'Simon Zhang'
    }
  },
 
  mounted() {
    this.initAjax()
  },
 
  computed: {
    login() {
      return this.$store.state.login
    },
    load() {
      return this.$store.getters.load
    }
  },
 
  methods: {
    initAjax() {
      this.$store.dispatch('initAjax')
    }
  }
}
```
