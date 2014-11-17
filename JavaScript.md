FantasyMedia JavaScript Style Guide
---

*Inspired by [Airbnb JavaScript style guide](https://github.com/airbnb/javascript) and made customized some rules*

## Table of Contents

1. [Types](#types)
    1. [Objects](#objects)
    2. [Arrays](#arrays)
    3. [String](#string)

## Types

- 变量声明

使用字面量声明变量：

```
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

    ```
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

- **复杂类型**: 操作变量的引用

    - `object`
    - `array`
    - `function`

    ```
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

### Object

- 不要使用JavaScript中的[保留字](http://es5.github.io/#x7.6.1)，例如：

```
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

```
var someStack = [];

// bad
someStack[someStack.length] = 'foo';

// good
someStack.push('foo');
```

- 使用Array#slice复制数组（[jsperf](http://jsperf.com/converting-arguments-to-an-array/7)）

```
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

### String

- 统一使用单引号，个别情况（例如以下）除外：

```
var foo = someVar.replace(/"/g, "''");
```

- 使用更容易阅读的`+`来链接过长的（一般约定为80）字符串：

```
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

```
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
