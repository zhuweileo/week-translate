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





