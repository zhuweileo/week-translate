# 在React中使用TypeScript

原文链接 https://simonknott.de/articles/Using-TypeScript-with-React.html

以下是原文翻译

----

这篇博客是基于我在[ React JS & React Native Bonn Meetup](https://www.meetup.com/React-JS-React-Native-Bonn-Meetup/events/262193621/)的一次演讲。博客主要目标是回答以下问题：

- 什么是TypeScript
- 如何配合React一起使用
- 我为什么（不）应该用它

# TypeScript是什么？

在 [typescriptlang.org](https://typescriptlang.org/) 网站上，对TypeScript（简写为TS）有如下描述：

>TypeScript 是一个能被编译为纯JS的类型化JS超集。

那么这意为着什么呢？首先，很明显的是TS是一种和JS有关的类型化编程语言。这一点是TS和JS主要区别，因为JS是一种动态类型语言。其次，TS可以被编译为纯JS，并可以在浏览器或Node.js环境执行。最后，最为JS的超集，意味着TS只会在严格兼容JS特性的前提下，添加自己的新特性。

让我们一起看个例子。既然TS是JS的超集，下面的代码对于两种语言都是可用的：

```js
function add(a, b) {
  return a + b;
}

const favNumber = add(31, 11);
```

我们早就知道的一点是，TS的特别之处是它的**类型**。将上面的代码加上**类型**之后，就得到了下面的代码。下面的代码现在是有效的TS代码，但对于JS来说是无效的。

```tsx
function add(a: number, b: number): number {
  return a + b;
}

const favNumber: number = add(31, 11);
```

通过加上这些注解，TS编译器可以通过检查你的代码帮你发现bug。例如，它可以发现不匹配的参数类型，或者在当前作用域外的变量引用。

TS编译是通过`tsc`CLI工具实现的，它可以对代码进行类型检查，如果全部通过，将会被编译为纯JS，被编译的代码和你的源码非常接近，只是没有了注解。

TS也支持基础的类型推断，这可以允许缺省函数的返回值类型，多数情况下也可以省略变量的类型，这样有助于简化代码：

```tsx
function add(a: number, b: number) {
  return a + b;
}

const favNumber = add(31, 11);
```

（代码第一行没有函数返回值类型，第五行没有变量类型）



让我们一起看一看在TS中都有哪些变量类型是可用的。期中的原始类型有 `boolean`, `string`, `number`, `Symbol`, `null`, `undefined` 和 `BigInt`  (是的， `BigInt` 好像是现代JS中的一个原始类型 😅)；`void`类型表示函数没有返回任何内容；`Array<T>`类型通常表示为`string[]`或`number[]`。当然这里也有环境特定的类型像，  `ReactElement`, `HTMLInputElement` 或 `Express.App`  ，这些类型不是所有项目都可用的，只对特定的项目可用。



既然React比较像纯JS，那么就能像对纯JS那样进行类型化。你仅需要使用`interface`声明组件的参数结构，然后用声明好的`interface`去注解你的组件参数。你不需要指明组件的返回值类型（`JSX.Element`），因为它可以从组件的返回表达式中被推导出来。

尽管JSX不是JS的一部分，TS仍然能够对JSX进行类型检查。这样就可以对你传入组件的参数进行可用性检测了。

当然你也可以对React的`class`类型组件使用TS：

```tsx
import * as React from "react";

interface TimerState {
  count: number;
}

class Timer extends React.Component<{}, TimerState> {
  
  state: TimerState = {
    count: 0
  }

  timerId: number | undefined = undefined;

  componentDidMount() {
    this.timerId = setInterval(
      () => {
        this.setState(
          state => ({
            count: state.count + 1
          })
        );
      },
      1000
    );
  }

  componentWillUnmount() {
    if (this.timerId) {
      clearInterval(this.timerId);
    }
  }

  render() {
    return (
      <p>Count: {this.state.count}</p>
    )
  }
}
```

当对`class`进行类型化的时候，你将类型参数传入继承了`React.Component`的类中。上面例子中，第一个传入的类型（一个空对象{}）对应组件的`props`。第二个传入的类型对应组件的`state`，例子中为一个number 类型的count。上面组件中还有一个实例属性`timerId`被初始化为`undefined`。



