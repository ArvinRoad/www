---
title: TypeScript | JavaScript 变量与常量
date: 2021/12/05 20:47:52
tags:
  - 程序
categories:
  - Script 
cover: https://th.bing.com/th/id/R.dc288c8099126dcd9dac931dba803328?rik=UT7Q8NHUjM78nw&riu=http%3a%2f%2fbloghoctap.com%2fwp-content%2fuploads%2f2017%2f01%2ftypescript.jpg&ehk=sAVZdoJHlKzU%2b7PmPVy5RFSx74EWpBmneW7oTdKbFvM%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin 
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
### 语句介绍

**输出语句**

document.write(); 只支持输出为字符串类型

- TypeScript

```typescript
document.write("Hello World");
```

- JavaScript

```javascript
document.write("Hello World");
```

> 可产生变化的输出语句（通过变量）：

- TypeScript

```typescript
// 声明变量(let或者Var)：
let PersonName = "Hello World";
PersonName = "你好，世界";
document.write(PersonName);
```

- JavaScript

```javascript
var PersonName = "Hello World";
PersonName = "你好，世界";
document.write(PersonName);
```

> const 常量无法改变

- TypeScript

```typescript
// 声明常量
const tmp = "Hello";
document.write(tmp);
```



- JavaScript（没有常量）

```javascript
var tmp = "Hello";
document.write(tmp);
```

> 声明类型限定（只能存储限定类型）

- TypeScript

```typescript
// 限定为字符串
var PersonName:string = "Hello";

// 报错：因为111不是字符串类型而是整型，需要修改成number（int）类型
var PersonName：number = "Hello"
PersonName = 111;
```



- JavaScript（没有类型限定）

```javascript
var PersonName = "Hello";
```

---

### 类型

> 在TypeScript | JavaScript 中，如果我们没有给予数据类型，它会根据你的数据自动判断类型。例如：最好写上

| 类型     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 枚举     | 自己定义一个属于自己的类型                                   |
| 联合类型 | 一个变量支持两种或多种类型                                   |
| number   | 整型，与 int 一样                                            |
| string   | 字符串，可以用 + 连接字符串或者连接其他类型，还有模板字符串 `` |
| boolean  | 布尔类型，只含两个字：ture（是）\| false（否）               |
| any      | 任何类型（可以包含任何类型）                                 |
| array    | 基础数组                                                     |



- TypeScript

```typescript
// 自动判定为number (int)类型
let tmp = 333;

// 自动将tmp转化为字符串类型
document.write(tmp + "");

// 如果tmp没有赋值则打印 特殊值：undefined (表示你声明了一个变量没有赋值)
let tmp:number;
document.write(tmp + "");

// 还有一个特殊值为 null 为空,表示我们主动赋值为空
tmp = null;
document.write(tmp + "");
```



- JavaScript

```javascript
var tmp = 333;
```

> 模板字符串

- TypeScript

```typescript
let tmp:number = 520;
document.write(`Hello ${tmp},
World`);
```

> any 可以包含任何类型

- TypeScript

```typescript
let tmp any = false;
tmp = Hello;
tmp = 123;
```

> array 基础数组

- TypeScript

`````typescript
let a:number = 1;
let b:number = 2;
let b:number = 3;
````

// 数组：首先类型相同，变量含意相同(例如关卡)
let a:number[] = [0,1,2,3];
doucument.write(`A的数组为${a}`)

// string 演示(个子类型都可以用数组表示)：
let Names:string[] = ["Hello","World"]
doucument.write(`Name的数组为${Names}`) 

// 当然也可以直接用
let a:number[] = [0,1,2,3];
let b:number = a[3];
doucument.write(`B的数值为${b}`)
`````

> 联合类型：一个变量支持两种或多种类型

- TypeScript

```typescript
let num:number|string = 0;
num = "Hello";

// 报错：不支持布尔类型需要添加
num = ture;
```

> 枚举

假设我们的游戏通常需要很多不同的颜色，我希望有一个值来表示颜色

- TypeScript

```typescript
// 普通方法 
let color:number = 0;	//0 白色 1 黑色
// 或者玩家的一个状态
let state:number = 0;	// 0站立 1 跑步 2 死亡 3 攻击

// 枚举
enum Color{
    red,
    blue,
    green
}
let color:Color = Color.red;
// 或者玩家的一个状态
enum State{
    idle,
    run,
    death,
    attack
}
let state:State = state.idle;
```

> 注释

```typescript
// 单行注释
/* 多行注释 */
```

> 类型验证

- TypeScript

`````typescript
let X = 10;
````
// 假设我没有定义X的类型(自动判定)，后面我写了很多行代码，忘记了X的类型，我们可以输出X的类型查看
document.write(typeof X);	// 输出类型

// 关于枚举
enum Color{
    red,
    blue,
    green
}

let color:Color = color.red;
document.write(typeof color);	// 输出为number
document.write(Color.red + "");	// 输出为1
// 枚举本身是采用整型记录的所以它的类型依旧为整型
enum Color{
    red = 2,
    blue,
    green = 3
}
`````

> 类型别名

- TypeScript

```typescript
Type NewNumber = number;
let num:NewNumber = 3;
document.write(num);	// 输出为3
document.write(typeof num);	//	输出l
// 这样我们就可以修改类型名称为自定义的了 我真想把number改成int
```