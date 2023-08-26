---
title: TypeScript 程序流程
date: 2021/12/06 15:22:52
tags:
  - 程序
categories:
  - Script 
cover: https://th.bing.com/th/id/R.dc288c8099126dcd9dac931dba803328?rik=UT7Q8NHUjM78nw&riu=http%3a%2f%2fbloghoctap.com%2fwp-content%2fuploads%2f2017%2f01%2ftypescript.jpg&ehk=sAVZdoJHlKzU%2b7PmPVy5RFSx74EWpBmneW7oTdKbFvM%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin 
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

### 运算符

**算术运算符**

| 运算符   | 说明         |
| -------- | ------------ |
| +        | 加号         |
| -        | 减号         |
| *        | 乘号         |
| /        | 除号         |
| %        | 取余（取模） |
| ++ \| -- | 自增 \| 自减 |

**比较运算符**

| 运算符 | 说明                       |
| ------ | -------------------------- |
| >      | 大于                       |
| <      | 小于                       |
| >=     | 大于等于                   |
| <=     | 小于等于                   |
| ==     | 等于                       |
| ===    | 等于（不仅值还包括类型）   |
| !=     | 不等于                     |
| !==    | 不等于（不仅值还包括类型） |

- TypeScript

```typescript
let num = 2 + 3;
let num = 2 - ( 3 + 3);

// 取余
let num = 10 % 3;
document.write(num);	// 输出为 1 取余等于（3*3 = 9）余 1

// ++自增
num = num + 1;
num ++;
++ num;
// 在输出中++是程序会先打印然后执行++操作
document.write(num++ + "");
document.write(num + "");

//如果要先++再使用可以采用
document.write(++num + "");

// 比较运算
let res:boolean = 5 > 3;
document.write(res);	// 输出为true
res = 5 < 3;
document.write(res);	// 输出为false
num = 5
res = num == 5;
document.write(res);	// 输出为true

num = 3;
num2 = "3";
res = num === num2;	// 输出为false
res = num != num2;	// 输出为ture
```

**逻辑运算符**

| 运算符               | 说明                 |
| -------------------- | -------------------- |
| &&                   | 逻辑与（并且 \| 又） |
| \|\|                 | 或                   |
| ! ture (或者! false) | 逻辑非（取反）       |

- TypeScript

```typescript
// 例如：10 > num >2; 但是我们程序不能这样写：
let res:boolean = null;
res = num > 3 && < 10;	// num大于3，小于10 结果才为ture 一个为false 则结果为 false

// 或者是 num > 10 num < 5;
res = num > 10 || num < 5; // 两个有一个为ture 则结果为 ture

// 取反
res = !(num > 10); //如果（num > 10）为ture 则结果为 false
```

**赋值运算符**

| 运算符 | 说明   |
| ------ | ------ |
| =      | 赋值   |
| +=     | 加赋值 |
| -=     | 减赋值 |
| *=     | 乘赋值 |
| /=     | 除赋值 |



- TypeScript

```typescript
let num：number = 3; // 将3赋值给num

num = num +3;	// 给num的值加3可以写成：
num += 3;
```

---

### 判断语句 | 条件控制语句

假设有一个值 Age 如果大于18就输出成年人 如果小于18就输出未成年人

- TypeScript

```typescript
// if 判断语句 if 表示如果 else 表示否则
let age = 20;
if(age > 18){
    document.write("成年人");
}else{
    docment.write("未成年人");
}

// 如果大于60为老年人
if( age > 60){
    document.write("老年人");
}else if(age > 18){
    doucument.write("成年人");
}else{
    doucument.write("未成年");
}

// 假设 0-59 是一般 60-79 是及格 80-100 是优秀
let score = 80;
if(score >= 0 && score < 60){
    document.write("一般");
}else if(score >= 60 && score < 80){
    document.write("及格");
}else if(score>=80&& score <=100){
    document.write("优秀");
}else{
    document.write("系统出错");
}

// 假设我们有一个num 如果num大于100 我们只允许给它100，否则就输出本身值
let num = 105;
if(num > 100){
    num = 100;
}
document.write(num);
// 当然我们可以采用三目运算更加快速的解决这个问题 (条件？值1:值2 如果为ture则赋值值1 否则赋值值2)
num = num > 100 ? 100 : num;

// 假设我们需要多种状态
enum State{
    idle,
    run,
    attack,
    die
}

let state:State = State.idle;

if(state == State.idle){
    document.write("站立");
}
// 我们可以采用选择结构语句(分支语句) switch:
switch(state){
    case State.idel:
        document.write("站立");
        break; // 到此结束 
    case State.run:
        document.write("跑步");
        break;
    case State.attack:
        document.write("攻击");
        break;
    default: //如果以上都没进来就执行：
        document.write("死亡");
}
```

---

### 循环控制语句

- TypeScript

```typescript
// while() 循环判断 我们先输出5个HelloWorld
let i:number = 0;
let num:number = 0;
while(i < 5){
   document.write("Hello World");
    i++;
}
document.write(num + "")

// 我们在游戏开发中通常有while做成死循环检测或执行游戏全生命周期的事件
while(ture){
	// 游戏中持久生命周期事件
}

// 假设我们要打印出100以内的所有偶数的和
let i:number = 0;
let num: number = 0;
while(i < 101){
    if(i % 2 == 0){
        num += i;
    }
    i++;
}
document.wirte(num +"");

// do ... while 循环 第一次无论如何就进行执行一次 再进行循环、判断
let i:number = 1;
do{
    document.write("do.while 循环");
}while(i < 0);

// 我们有一个数组代表人名,我们需要将数组内容逐步打印出来：
let names:string[] = ["张三","李四","王五"]
document.write(names[0]);	// 这样的打印方法是效率很低的，且不好管理
// 我们用遍历方式
let i :number = 0;
while(i < 3){
    document.write(name[i]);
    i++;
}

// 当然还要一个循环方式更加便捷，省去我们在外面创建变量和自增
for(let i = 0;i < 3; i++;){
    document.write(names[i]);
}

// 当然在后面为了更加便捷还有一种for循环 它会遍历数组直到全部完成
for(let tmpName of nams){
    document.write(tmpName);
}

// in 不会输出遍历数组的值而是遍历输出数组的索引
for(let index in names){
    document.write(indx);	// 输出 0 1 2
}

// 当然我们可以在循环中退出来
for(let i =0; i < 10; i++){
    if(i > 5){
        break; //跳出for循环 这个方法可以用在所有循环，但是注意它只会跳出自己最近的循环，如果是嵌套循环，它只能跳出第一层
    }
}
```

