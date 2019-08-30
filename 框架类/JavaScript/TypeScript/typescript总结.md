### 5分钟上手TypeScript教程
1、类型注解：这是一种轻量级的为函数或变量添加约束的方式。

2、接口：interface 

3、类：class

4、class和interface可以一起共作，可以自行决定抽象的级别。

5、在构造函数的参数上使用public等同于创建了同名的成员变量。

6、webpack的项目中如果使用了typescript：

（1）需执行：
    npm install --save-dev typescript awesome-typescript-loader source-map-loader

（2）awesome-typescript-loader可以让Webpack使用TypeScript的标准配置文件 tsconfig.json编译TypeScript代码。

（3）source-map-loader使用TypeScript输出的sourcemap文件来告诉webpack何时生成 自己的sourcemaps，方便调试代码。

7、tsconfig.json

    {
        "compilerOptions": {
            "outDir": "./dist/",
            "sourceMap": true,
            "noImplicitAny": true,
            "module": "commonjs",
            "target": "es5",
            "jsx": "react"
        },
        "include": [
            "./src/**/*"
        ]
    }

8、webpack.config.json 中的externals：可以避免把所有的React都放在一个文件里，因为会增加编译时间并且浏览器还能够缓存没有发生改变的库文件。

    externals: {
        "react": "React",
        "react-dom": "ReactDOM"
    }


### 手册指南
#### 基本类型
1、布尔值 boolean

2、数字 number

3、字符串 string

4、数组

（1）可在元素类型后面接上[]

（2）可使用数组泛型：Array<元素类型>

5、元组Tuple
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
    
    let x: [string, number];

6、枚举enum
默认情况下，从0开始为元素编号。当然也可以手动指定成员的数组。

    enum Color {Red = 1,Green = 2,Blue = 4,Pink = 8}
    let c: Color = Color.Pink;
    let colorName: string = Color[1];

    console.log(c,colorName);//8,"Red"

7、any
不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。

    let notSure: any = 57
    notSure = "maybe a string instead";
    notSure = false;

8、void
表示没有任何类型。当一个函数没有返回值时，通常会见到其返回值类型是void。

    function warnUser(): void {
        console.log("This is my warnning message");
    }

9、null 和 undefined
（1）默认情况下，null和undefined是所有类型的子类型。
（2）如果指定了 --strictNullChecks 标记，null和undefined只能赋值给void和它们自己。

    let u: undefined = undefined;
    let n: null = null;

10、never
（1）表示那些永不存在的值的类型。
（2）never类型是任何类型的子类型，也可以赋值给任何类型。
（3）没有类型是never的子类型或者可以赋值给never类型（除了never本身之外）。即使 any也不可以赋值给never。
（4）never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
（5）变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

    // 返回never的函数必须存在无法达到的终点
    function error(message: string): never {
        throw new Error(message);
    }

    // 推断的返回值类型为never
    function fail() {
        return error("Something failed");
    }

    // 返回never的函数必须存在无法达到的终点
    function infiniteLoop(): never {
        while (true) {
        }
    }

11、object
表示非原始类型。可以更好的表示像Object.create这样的API。

    declare function create(o: object | null): void;
    create({ prop: 0 }); // OK
    create(null); // OK

12、类型断言
（1）可以用尖括号语法，也可以用as语法。
（2）表示使用者很清楚地知道当前数据是什么类型的。

    //方式1
    let someValue: any = "this is a string";
    let strLength: number = (<string>someValue).length;

    //方式2，若使用JSX，则只能使用此方式
    let someValue: any = "this is a string";
    let strLength: number = (someValue as string).length;


#### 变量声明
1、经典的一个demo

    for (var i = 0; i < 10; i++) {
        setTimeout(function() { console.log(i); }, 100 * i);
    }

    上面的这个demo，很显然会打印出来10个10，并不是我们最初设想的从0到9。
    这是因为 setTimeout函数是在for循环结束后执行，最后的i已经变成了10。

    如何解决这个问题呢？
    (1)可以借助 **立即执行函数** 来捕获每次循环时的i值。

    for (var i = 0; i < 10; i++) {
        (function(i) {
            setTimeout(function() { console.log(i); }, 100 * i);
        })(i);
    }

    (2)将var改成let即可。

2、let：块级作用域
块级作用域是不能在包含它的块之外被访问的。
同一块作用域，不能重复声明变量的。

3、解构
（1）数组解构
    let [first, ...rest] = [1, 2, 3, 4];

（2）对象解构
    let o = {
        a: "foo",
        b: 12,
        c: "bar"
    };
    let { a, ...passthrough } = o;
    let total = passthrough.b + passthrough.c.length;

（3）属性重命名
    //将a重命名为newName1,并指定类型为string；
    //将b重命名为newName2,并指定类型为number；
    let { a: newName1, b: newName2 }:{a:string,b:number} = o;

（4）默认值
    function keepWholeObject(wholeObject: { a: string, b?: number }) {
        let { a, b = 1001 } = wholeObject;
    }

（5）属性解构
    type C = { a: string, b?: number }
    function f({ a, b }: C): void {
        // ...
    }

（6）展开
    a.与解构是相反的。
    b.允许将一个数组展开为另一个数组，或者将一个对象展开为另一个对象。
    c.展开操作时，是浅拷贝，并不会将原数组或原对象的值进行修改。


#### 接口 interface
1、类型检查器不会检查值的顺序，只要相应的属性存在并且类型是对的即可。

2、可选属性
（1）带有可选属性的接口，只要在属性名字的后面带上？即可。
（2）可选属性的好处有：
    a.可以对可能存在的属性进行预定义。
    b.可以捕获引用了不存在的属性时的错误。

（3）demo：

    interface SquareConfig {
        color?: string;
        width?: number;
    }

    function createSquare(config: SquareConfig): { color: string; area: number } {
        let newSquare = {color: "white", area: 100};
        if (config.clor) {
            // Error: Property 'clor' does not exist on type 'SquareConfig'
            newSquare.color = config.clor;
        }
        if (config.width) {
            newSquare.area = config.width * config.width;
        }
        return newSquare;
    }

    let mySquare = createSquare({color: "black"});
    
3、只读属性
（1）只能在创建时修改值。
（2）只要在属性名前设置 readonly 即可。
（3）ReadonlyArray<T>类型：
    a.可以将可变的方法都去掉，确保数组创建后不会被修改。
    b.但是可以用类型断言重写。
（4）readonly 与 const：作为变量使用const，作为属性使用readonly。
（5）demo

    let a: number[] = [1, 2, 3, 4];
    let ro: ReadonlyArray<number> = a;
    a = ro as number[];

4、函数类型
（1）就可以设置函数的入参和返回值的类型。
（2）demo
    interface SearchFunc {
    (source: string, subString: string): boolean;
    }

5、可索引的类型
（1）具有一个索引签名，描述了对象索引的类型，还有相应的索引返回值类型。
（2）索引类型支持两种：字符串和数字。数字索引的返回值必须是字符串索引返回值类型的子类型。 
（3）建议给索引签名设置给只读，防止给索引赋值。
（4）demo
    interface ReadonlyStringArray {
        readonly [index: number]: string;
    }
    let myArray: ReadonlyStringArray = ["Alice", "Bob"];

6、类类型
（1）分为静态部分和实例部分
（2）demo

    interface ClockConstructor {
        new (hour: number, minute: number): ClockInterface;
    }
    interface ClockInterface {
        tick();
    }

    function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
        return new ctor(hour, minute);
    }

    class DigitalClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("beep beep");
        }
    }
    class AnalogClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("tick tock");
        }
    }

    let digital = createClock(DigitalClock, 12, 17);
    let analog = createClock(AnalogClock, 7, 32);

7、继承接口
（1）一个接口可以继承多个接口
（2）demo

    interface Shape {
        color: string;
    }

    interface PenStroke {
        penWidth: number;
    }

    interface Square extends Shape, PenStroke {
        sideLength: number;
    }

    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;
    square.penWidth = 5.0;

8、混合类型

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

9、接口继承类
（1）当接口继承了一个类类型时，它会继承类的成员但不包括其实现。
（2）demo
    class Control {
        private state: any;
    }

    interface SelectableControl extends Control {
        select(): void;
    }

    class Button extends Control implements SelectableControl {
        select() { }
    }

    class TextBox extends Control {
        select() { }
    }

    // 错误：“Image”类型缺少“state”属性。
    class Image implements SelectableControl {
        select() { }
    }

    class Location {

    }


#### 类
1、

#### 函数

#### 泛型

#### 枚举

