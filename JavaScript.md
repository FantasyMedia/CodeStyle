FantasyMedia JavaScript Style Guide
---

*Inspired by [Airbnb JavaScript style guide](https://github.com/airbnb/javascript) and made customized some rules*

## Table of Contents

1. [Types](#types)
    - [Object](#object)
    - [Array](#array)
    - [String](#string)
    - [Function](#function)
2. [Properties](#properties)
3. [Variables](#variables)
4. [Equality](#equality)
5. [Blocks](#blocks)
6. [Comments](#comments)
7. [Whitespace](#whitespace)
8. [Punctuation](#punctuation)
    - [Commas](#commas)
    - [Semicolons](#Semicolons)

## Types

- 变量声明

使用字面量声明变量：

```javascript
// 对象
var obj = {};

// 数组
var arr = [];

// 字符串
var str = '';
```

- **原始类型**: 直接操作变量的值：

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

- **复杂类型**: 操作变量的引用

    - `object`
    - `array`
    - `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

[↑ back to top](#table-of-contents)

### Object

- 不要使用JavaScript中的[保留字](http://es5.github.io/#x7.6.1)，例如：

```javascript
// bad
var superman = {
    default: {
        clark: 'kent'
    },
    private: true
}

// good
var superman = {
    defaults: {
        clark: 'kent'
    },
    hidden: true
}
```

### Array

- 在不知道数组长度的情况下，使用Array#push

```javascript
var someStack = [];

// bad
someStack[someStack.length] = 'foo';

// good
someStack.push('foo');
```

- 使用Array#slice复制数组（[jsperf](http://jsperf.com/converting-arguments-to-an-array/7)）

```javascript
var len = items.length,
    itemsCopy = [],
    i;

// bad
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```

[↑ back to top](#table-of-contents)

### String

- 统一使用单引号，个别情况（例如以下）除外：

```javascript
var foo = someVar.replace(/"/g, "''");
```

- 使用更容易阅读的`+`来链接过长的（一般约定为80）字符串：

```javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
```

- 使用程序控制语句来拼接字符串的时候，使用Array#join而不是`+`

*以下例子的情况经常会在我们拼装Ajax返回数据、塞到原有元素中的时候出现(不使用template)的情况时*

```javascript
var items,
    messages,
    length,
    i;

messages = [{
  state: 'success',
  message: 'This one worked.'
}, {
  state: 'success',
  message: 'This one worked as well.'
}, {
  state: 'error',
  message: 'This one did not work.'
}];

length = messages.length;

// bad
function inbox(messages) {
  items = '<ul>';

  for (i = 0; i < length; i++) {
    items += '<li>' + messages[i].message + '</li>';
  }

  return items + '</ul>';
}

// good
function inbox(messages) {
  items = [];

  for (i = 0; i < length; i++) {
    items[i] = messages[i].message;
  }

  return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
}
```

[↑ back to top](#table-of-contents)

### Function

- 函数表达式

```javascript
// anonymous function expression
var anonymous = function() {
  return true;
};

// named function expression
var named = function named() {
  return true;
};

// immediately-invoked function expression (IIFE)
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

- 不要将函数参数命名为`arguments`，这会覆盖每个函数自带的`arguments`的优先级:

```javascript
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```

[↑ back to top](#table-of-contents)

## Properties

- 统一使用`.`或者`[]`获取对象的属性：

**[StackOverFlow讨论](http://stackoverflow.com/questions/4968406/javascript-property-access-dot-notation-vs-brackets)**

```javascript
var luke = {
  jedi: true,
  age: 28
};

// 两者都可以获得对象的属性，但是在代码中需要统一风格
var isJedi = luke['jedi'];
var isJedi = luke.jedi;
```

- 使用`[]`获取带变量的属性值：

这也是上一条中使用`[]`的好处之一，可以获取*带变量的属性*

```javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```

[↑ back to top](#table-of-contents)

## Variables

- 使用`var`关键字声明变量，避免产生无用的全局变量进而影响全局的命名空间

```javascript
// bad
foo = 'bar';

// good
var foo = 'bar';
```

- 在作用域的顶部声明变量

```javascript
function foo () {
    var num = 1;
    var test = true;

    //..some stuff..
}

```

- 同时声明多个变量时，可以采取以下两种形式，但是在自己的代码中需要统一风格

```javascript
// 使用一个var
var foo = 'bar',
    isSelected = true,
    num = 0;

// 使用多个var
var foo = 'bar';
var isSelected = true;
var num = 0;
```

关于第一种形式还有以下几个注意点：

- **千万**不要写成以下形式：

```javascript
var foo = 'bar'
  , isSelected = true
  , num = 0;
```

- 每一行只声明一个变量:

```javascript
// good
var foo = 'bar',
    isSelected = true,
    num = 0,
    i;

// bad
var foo = 'bar',
    isSelected = true,
    num = 0, i;
```

- 可以*适当的*将`=`对齐:

```javascript
var foo        = 'bar',
    isSelected = true,
    num        = 0;
```

[↑ back to top](#table-of-contents)

## Equality

- 使用`===`和`!==`，而不是`==`和`!=`

需要知道自己在使用比较运算符的时候，运算符的两边是什么类型

## Blocks

- 使用`{}`，即使里面只有一行代码：

```javascript
if (test) {
    return false;
}
```

以下风格亦是可以接受的：

```javascript
if (test) return false;
```

- 函数体必须使用`{}`：

```javascript
function test () {
    return false;
}
```

[↑ back to top](#table-of-contents)

## Comments

使用规范的注释能够帮助队友更好的阅读你的代码，这一点在协作开发的时候相当重要：

- 文件级和函数级的注释风格：

```javascript
/**
 * This is a test function
 *
 * @param  Int val
 * @return Boolean
 */
function test (val) {
    return val === 1;
}
```

- 单行注释：

单行注释务必写到单独的一行，如：

```javascript
// This is for lock
var lock = false;
```

而不要将注释写到语句之后：

```javascript
var lock = false; // This is for lock
```

[↑ back to top](#table-of-contents)

## Whitespace

- 使用4个或者2个空格组成的soft tabs

> 为什么是4个或者2个？

由于开发者之间的习惯差异比较大，所以不能强制要求使用4个或者2个空格，但是**必须**保证自己的代码中的风格是一致的。

关于这一点的设置在大部分编辑器、IDE中都可以直接设置，例如：

1. Atom编辑器可以直接勾选soft tabs选项
2. Sublime Text可以通过`"tab_size": 4, "translate_tabs_to_spaces": true,`进行设置

其他IDE中可以在各自的首选项中进行设置。

- 在`{`之前添加1个空格：

函数声明：

```javascript
// Bad
function test (){
    //
}
// Good
function test () {
    //
}
```
- 参数之间添加1个空格：

```javascript
test(a, b, {
    c: d
});
```

- 操作符与变量、值之间添加空格：

```javascript
// Bad
var foo=1+2;

// Good
var foo = 1 + 2;
```

[↑ back to top](#table-of-contents)

## Punctuation

### Commas

在[Variables](#variables)一节中已经提及了一次`,`的规范，接下来做一点点补充

- 不要使用多余的`,`

```javascript
// Wrong
var cold = {
    name: 'char',
    age: '24',
    gender: 'male',
}

// Right
var cold = {
    name: 'char',
    age: '24',
    gender: 'male'
}
```

### Semicolons

必须**正确**使用分号

- 变量、函数声明：

```javascript
var foo = 'bar';

var test = function () {
    return false;
};
```

- 函数调用：

以下是正确使用分号的方式：

```javascript
test();

(function () {
    return foo = 'bar';
})();

;(function () {
    return bar = 'foo';
})();
```

[↑ back to top](#table-of-contents)
