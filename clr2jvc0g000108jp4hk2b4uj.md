---
title: "When to use the Nullish Coalescing operator?"
seoTitle: "When to use the Nullish Coalescing operator?"
seoDescription: "The Nullish Coalescing operator ?? is a logical operator (similar to && and ||) that  treats only null and undefined as falsy values. In other words ?? retu"
datePublished: Sat Jan 06 2024 21:01:16 GMT+0000 (Coordinated Universal Time)
cuid: clr2jvc0g000108jp4hk2b4uj
slug: when-to-use-the-nullish-coalescing-operator
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703958417234/d5106c7f-ed0e-410b-ad7b-72b50cef60c5.jpeg
tags: javascript

---

### Introduction

The Nullish Coalescing operator `??` is a logical operator (similar to `&&` and `||`) that treats only `null` and `undefined` as falsy values. In other words `??` returns the first operand if it is not `null` or `undefined`. Otherwise, it'll return the second operand.

```javascript
// returns a if a is neither null/undefined, else returns b.
result = a ?? b;
```

the above expression without using `??`

```javascript
result = (a !== null && a !== undefined) ? a : b;
```

> ⚠️Warning: Since this is a recent addition to the language(ES11 2020). You might have to add **polyfills** for Odler browser versions. (or) check if it is compatible with your Node.js version before breaking the production pipeline like me😬.

### Alternatives of Nullish Coalescing (before ES11)

Earlier when one wanted to assign a default value to a variable, it was a common practice to use the the logical OR (`||`) operator.

```javascript
let input, player = {};
// input is never assigned any value so it is still undefined
const player.name = input || "john doe";
```

But the problem with `||` is, it coerces the left-hand-side operand to a boolean and does not return any falsy value including `0`, `''`, `NaN`, `false`, etc.

```javascript
const score = 0, level = "", player = {};

player.score = score || 20;
player.level = level || "expert";
console.log(player.score); // 20 and not 0
console.log(player.level); // "expert" and not ""
```

What if you also want to consider `0`, `''` (or) `NaN` as valid values. This is where the Nullish Coalescing operator comes into the picture. It only returns the second operand when the first one is either `null` or `undefined`.

```javascript
const errorMsg = ""; // An empty string (which is a falsy value)

let isError = !!(errorMsg || "Unknown error occurred!!");
console.log(isError); // true

isError = !!(errorMsg ?? "Unknown error occurred!!");
console.log(isError); // false
```

### Usage with Optional Chaining operator (`?.`)

Similar to `??` the optional chaining operator (`?.`) also considers `null` and `undefined` as specific values. which is useful to access a property of an object which may be `null` or `undefined`. Combining them, you can safely access a property of an object which may be nullish and provide a default value if it is.

```javascript
const player = { name: "jon snow" };

console.log(player.name?.toUpperCase() ?? "unknown"); // "JON SNOW"
console.log(player.age?.toUpperCase() ?? "unknown"); // "unknown"
```

### How not to use it?

For safety reasons, it is forbidden to use `??` with `&&` and `||`. Unless you specify the presidence with explicit parenthesis.

```javascript
// Syntax error ❌
let x = 1 && 2 ?? 3;
// Do this instead ✅
let x = (1 && 2) ?? 3;
```

> Note: The presidence of `??` is same as that of `||`. they both equal `3` in [MDN table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#table)

### Conclusion

In conclusion the common use case of `??` is to provide a default value when the first operand is either `null` or `undefined`. Unlike `||` which returns the second operand even when the first operand is `0`, `Nan`, `''` or `false` in addition to `null` and `undefined`.