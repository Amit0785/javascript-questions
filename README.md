# javascript-questions
I would prepare you for a **4+ year React developer moving toward Node/Express/MongoDB**, with an interviewer who has **1 - 4 years of experience and architect-level exposure**.

For that interviewer, JavaScript questions will likely go beyond "what does `map()` do?" They may ask **why**, **what happens internally**, **edge cases**, and then give you code and ask you to predict the output.

Below is the JavaScript question set I would use.

# JavaScript Interview Questions — 4+ Years + Deep Level

## Level 1 — Core JavaScript

### Variables & Scope

Let’s go through each of those concepts step by step, then explain why your specific code throws an error:

---

## 🔑 Difference between `var`, `let`, and `const`
- **`var`**
  - Function-scoped.
  - Hoisted and initialized as `undefined`.
  - Can be redeclared and reassigned.
- **`let`**
  - Block-scoped.
  - Hoisted but **not initialized** (exists in the Temporal Dead Zone until its declaration line).
  - Can be reassigned but not redeclared in the same scope.
- **`const`**
  - Block-scoped.
  - Must be initialized at declaration.
  - Cannot be reassigned (though objects/arrays can be mutated).

---

## 🔑 Function Scope vs Block Scope
- **Function scope**: Variables declared with `var` are accessible throughout the entire function, regardless of block boundaries.
- **Block scope**: Variables declared with `let` and `const` are only accessible inside the `{ ... }` block where they are defined.

```js
function demo() {
  if (true) {
    var x = 1;   // function-scoped
    let y = 2;   // block-scoped
  }
  console.log(x); // ✅ 1
  console.log(y); // ❌ ReferenceError
}
```

---

## 🔑 Lexical Scope
- Scope is determined by **where code is written**, not where it’s executed.
- Inner functions can access variables from their outer functions.

```js
function outer() {
  let a = 10;
  function inner() {
    console.log(a); // ✅ 10 (lexical scope)
  }
  inner();
}
outer();
```

---

## 🔑 Temporal Dead Zone (TDZ)
- The period between entering a scope and the actual variable declaration where `let` and `const` exist but are **not accessible**.
- Accessing them before initialization throws a **ReferenceError**.

```js
console.log(b); // ❌ ReferenceError (TDZ)
let b = 20;
```

---

## ❌ Why does this throw an error?
```js
console.log(a);
let a = 10;
```

- `a` is hoisted but **not initialized**.
- Between the start of the scope and the line `let a = 10`, `a` is in the **Temporal Dead Zone**.
- Attempting to access it before initialization results in a **ReferenceError**.

---

✅ In short:  
- `var` → hoisted, initialized as `undefined`.  
- `let` / `const` → hoisted but uninitialized, stuck in TDZ until declared.  
- Your code fails because `a` is accessed while still in the TDZ.  

---

### 6. Why does this behave differently?

For:

```js
console.log(a);
var a = 10;
```

the output is:

```text
undefined
```

It does **not** throw a `ReferenceError`.

The reason is **`var` hoisting**.

JavaScript effectively handles the code in two conceptual stages:

**Creation phase:**

```js
var a;
```

The variable `a` is created and initialized with `undefined`.

**Execution phase:**

```js
console.log(a); // undefined
a = 10;
```

So you can think of the original code as behaving roughly like:

```js
var a;
console.log(a);
a = 10;
```

> Important: JavaScript doesn't literally move the line `var a = 10` to the top. Rather, the binding for `a` is created before the code executes.

---

### 7. What exactly happens during JavaScript's creation/execution phases?

A simplified mental model is:

#### 1. Creation phase

Before executing your code, JavaScript's execution context is prepared.

For example:

```js
console.log(a);
var a = 10;

function greet() {
  console.log("Hello");
}
```

During the creation phase, JavaScript roughly sets up:

```text
a      → undefined
greet  → function object
```

So:

* `var` declarations are created and initialized to `undefined`.
* Function declarations are created and initialized with the actual function.
* `let` and `const` bindings are also created, but they remain **uninitialized** until execution reaches their declaration. This is the **Temporal Dead Zone (TDZ)**.

#### 2. Execution phase

JavaScript then executes statements from top to bottom:

```js
console.log(a); // undefined
var a = 10;

greet();        // Hello

function greet() {
  console.log("Hello");
}
```

Conceptually:

```text
Creation phase:
    a      → undefined
    greet  → function

Execution phase:
    console.log(a)  → undefined
    a = 10
    greet()          → "Hello"
```

### Compare `var`, `let`, and `const`

```js
console.log(a);
var a = 10;        // undefined
```

```js
console.log(b);
let b = 10;        // ReferenceError
```

```js
console.log(c);
const c = 10;      // ReferenceError
```

The difference is that `let` and `const` are in the **TDZ** between entering the scope and reaching their declaration.

A useful interview mental model is:

```text
             Creation phase
                   ↓
       ┌─────────────────────┐
       │ var a → undefined   │
       │ let b → uninitialized│
       │ const c → uninitialized│
       │ function → function │
       └─────────────────────┘
                   ↓
             Execution phase
                   ↓
       Code runs top → bottom
```

One nuance: **“creation phase” and “execution phase” are useful teaching models**, not necessarily literal two passes over the source code. The ECMAScript specification describes this in terms of execution contexts, environment records, declaration instantiation, and evaluation.


---

# 2. Hoisting — Deep

8. What is hoisting?

Hoisting in JavaScript is the behavior where JavaScript processes declarations before executing the code in a scope.

9. Are `let` and `const` hoisted?

let and const are hoisted, but they are not initialized until execution reaches their declaration.
They remain in the Temporal Dead Zone (TDZ) from the beginning of their scope until the declaration is executed.

10. Predict the output:

```js
console.log(a);

var a = 10;

function test() {
  console.log(a);
}

test();
```

11. Predict:

```js
var x = 10;

function test() {
  console.log(x);
  var x = 20;
}

test();
```

12. Why does it print `undefined` rather than `10`?

13. Function declaration vs function expression hoisting?

```js
foo();

function foo() {
  console.log("foo");
}
```

vs.

```js
foo();

const foo = function () {
  console.log("foo");
};
```

---

# 3. Closures ⭐⭐⭐

This is **very important** for a senior JavaScript interview.

14. What is a closure?

15. Why does a closure remember variables after the outer function has finished?

16. Implement a counter using closure:

```js
const counter = createCounter();

counter();
counter();
counter();
```

Expected:

```text
1
2
3
```

17. What is the output?

```js
function outer() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

const fn1 = outer();
const fn2 = outer();

fn1();
fn1();
fn2();
fn1();
```

18. Explain why `fn1` and `fn2` don't share the same `count`.

19. What are practical uses of closures?

20. Can closures cause memory leaks?

21. How would you prevent unnecessary memory retention caused by closures?

---

# 4. Event Loop ⭐⭐⭐

This is one of the most important areas for your Node.js transition.

22. Explain the JavaScript event loop.

23. Explain:

```text
Call Stack
Web APIs / Node APIs
Callback Queue
Microtask Queue
Event Loop
```

24. What is the difference between **microtasks** and **macrotasks**?

25. Predict the output:

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

console.log("4");
```

Answer:

```text
1
4
3
2
```

26. Why does Promise execute before `setTimeout`?

---

### Deep Event Loop Question

27. Predict:

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");

  Promise.resolve().then(() => {
    console.log("D");
  });
});

console.log("E");
```

28. What happens if a microtask continuously creates another microtask?

29. Can microtasks starve the event loop?

30. Is JavaScript actually single-threaded?

31. If JavaScript is single-threaded, how can Node.js handle thousands of requests?

---

# 5. `this` ⭐⭐⭐

32. What is `this`?

33. How is `this` determined?

34. Explain `this` in:

```js
const obj = {
  name: "John",

  greet() {
    console.log(this.name);
  }
};

obj.greet();
```

35. What happens here?

```js
const greet = obj.greet;

greet();
```

36. Difference between:

```js
call()
apply()
bind()
```

37. Implement your own `bind()`.

38. Why don't arrow functions have their own `this`?

39. Predict:

```js
const obj = {
  name: "John",

  normal: function () {
    console.log(this.name);
  },

  arrow: () => {
    console.log(this.name);
  }
};

obj.normal();
obj.arrow();
```

40. What does `this` point to inside a class?

---

# 6. Prototypes & Inheritance ⭐⭐⭐

41. What is a prototype?

42. What is the prototype chain?

43. Explain:

```js
const obj = {};
```

What is its prototype?

44. What is:

```js
Object.prototype
```

45. Difference between:

```js
__proto__
prototype
```

46. Explain:

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(this.name);
};

const p = new Person("John");

p.sayHello();
```

47. Where does `sayHello()` actually live?

48. Why doesn't every object instance get a separate copy of `sayHello()`?

49. Explain inheritance using prototypes.

50. Difference between ES6 classes and prototype-based inheritance.

---

# 7. Objects & References ⭐⭐⭐

51. What is the difference between primitive and reference types?

52. Explain:

```js
let a = 10;
let b = a;

b = 20;

console.log(a);
```

vs.

```js
const a = { value: 10 };
const b = a;

b.value = 20;

console.log(a.value);
```

53. Why does:

```js
{} === {}
```

return `false`?

54. Explain shallow copy.

55. Explain deep copy.

56. Difference between:

```js
Object.assign({}, obj)
```

and

```js
{ ...obj }
```

57. Why are both shallow copies?

58. What problems can occur when using:

```js
JSON.parse(JSON.stringify(obj))
```

for deep cloning?

59. What is `structuredClone()`?

---

# 8. Array Methods ⭐⭐⭐

You specifically asked about these earlier, so expect questions around them.

60. Difference between:

```text
map
filter
forEach
reduce
find
findIndex
some
every
```

61. `map()` vs `forEach()`?

62. `map()` vs `reduce()`?

63. `find()` vs `filter()`?

64. `some()` vs `every()`?

65. `includes()` vs `indexOf()`?

66. `slice()` vs `splice()` ⭐

67. `sort()` vs `toSorted()`?

68. Which array methods mutate the original array?

69. What is wrong with:

```js
const numbers = [10, 2, 5, 1];

numbers.sort();
```

70. Explain:

```js
numbers.sort((a, b) => a - b);
```

---

# 9. Reduce — Deep

An architect-level interviewer may give you problems rather than ask "what is reduce?"

71. Calculate sum:

```js
[1, 2, 3, 4]
```

using `reduce()`.

72. Find maximum using `reduce()`.

73. Count occurrences:

```js
["a", "b", "a", "c", "b", "a"]
```

Expected:

```js
{
  a: 3,
  b: 2,
  c: 1
}
```

74. Convert an array to an object indexed by ID.

75. Group users by department.

76. Implement `groupBy()`.

---

# 10. Functions — Deep

77. Function declaration vs function expression?

78. Arrow function vs normal function?

79. What are first-class functions?

80. What is a higher-order function?

81. What is a callback?

82. What is currying?

83. Implement:

```js
sum(1)(2)(3)
```

84. What is partial application?

85. Currying vs partial application?

86. What is an IIFE?

87. Why were IIFEs commonly used before ES modules?

---

# 11. Rest vs Spread

88. Difference between:

```js
...
```

when used as rest and spread.

89. Example:

```js
function sum(...numbers) {}
```

90. Example:

```js
const newArray = [...arr];
```

91. Can you use spread to make a deep copy?

**Answer: No.**

It's shallow.

---

# 12. Destructuring

92. Explain array destructuring.

93. Explain object destructuring.

94. What is the difference between:

```js
const { name } = user;
```

and

```js
const { name: userName } = user;
```

95. Default values:

```js
const { name = "Unknown" } = user;
```

96. Nested destructuring.

---

# 13. Equality ⭐

97. Difference between:

```js
==
===
```

98. Why is:

```js
0 == false
```

true?

99. Why is:

```js
0 === false
```

false?

100. What is type coercion?

101. Explain:

```js
null == undefined
```

and

```js
null === undefined
```

102. Predict:

```js
[] == false
```

103. Predict:

```js
"" == false
```

These questions test whether you actually understand JavaScript coercion.

---

# 14. Promise ⭐⭐⭐

Very important for Node.

104. What is a Promise?

105. Promise states?

```text
pending
fulfilled
rejected
```

106. Difference between:

```js
.then()
.catch()
.finally()
```

107. Difference between:

```js
Promise.all()
Promise.allSettled()
Promise.race()
Promise.any()
```

108. What happens if one Promise fails inside `Promise.all()`?

109. When would you use `Promise.allSettled()`?

110. Implement a Promise-based retry mechanism.

111. What happens here?

```js
async function test() {
  return 10;
}

console.log(test());
```

Why isn't it `10`?

---

# 15. Async/Await ⭐⭐⭐

112. Is `async/await` synchronous or asynchronous?

113. What does an `async` function always return?

114. Difference between:

```js
await promise1;
await promise2;
```

and:

```js
await Promise.all([
  promise1,
  promise2
]);
```

115. Which is faster and why?

116. Find the problem:

```js
async function getData() {
  const users = await getUsers();
  const products = await getProducts();
  const orders = await getOrders();
}
```

If all three requests are independent, how could you improve it?

117. Rewrite using:

```js
Promise.all()
```

---

# 16. Error Handling

118. Difference between:

```js
throw
try/catch
Promise rejection
```

119. Does `try/catch` catch errors inside asynchronous callbacks?

For example:

```js
try {
  setTimeout(() => {
    throw new Error("Failed");
  }, 1000);
} catch (error) {
  console.log("Caught");
}
```

Will it catch the error?

Why?

120. How does `async/await` error handling work?

---

# 17. Debounce vs Throttle ⭐⭐⭐

Very relevant to React/frontend.

121. What is debounce?

122. What is throttle?

123. When would you use debounce?

Examples:

```text
Search box
API calls
```

124. When would you use throttle?

Examples:

```text
Scroll
Resize
Mouse movement
```

125. Implement debounce from scratch.

126. Implement throttle from scratch.

127. How would you cancel a pending debounce?

---

# 18. Event Delegation

128. What is event bubbling?

129. What is event capturing?

130. What is event delegation?

131. Why is event delegation useful?

Example:

```html
<ul id="users">
  <li>User 1</li>
  <li>User 2</li>
  <li>User 3</li>
</ul>
```

Instead of adding listeners to every `<li>`, how could you handle clicks using the `<ul>`?

132. Difference between:

```js
event.target
event.currentTarget
```

---

# 19. Modules

133. CommonJS vs ES Modules?

```js
require()
module.exports
```

vs.

```js
import
export
```

134. Default export vs named export?

135. What is tree shaking?

136. What is circular dependency?

137. How can circular dependencies cause problems?

---

# 20. Memory Management ⭐⭐⭐

This is a good architect-level topic.

138. How does JavaScript manage memory?

139. What is garbage collection?

140. What is a memory leak?

141. Common causes of memory leaks?

```text
Unremoved event listeners
Timers
Closures
Global variables
Large caches
Detached DOM nodes
```

142. How would you investigate a memory leak in a frontend application?

143. What is WeakMap?

144. Difference between:

```text
Map
WeakMap
```

145. Why can't you iterate over a WeakMap?

---

# 21. Map, Set, WeakMap, WeakSet

146. `Object` vs `Map`?

147. `Array` vs `Set`?

148. When would you use `Set`?

Example:

```js
const arr = [1, 2, 2, 3, 3, 4];

const unique = [...new Set(arr)];
```

149. `Map` vs `WeakMap`?

150. `Set` vs `WeakSet`?

---

# 22. Symbols

151. What is a `Symbol`?

152. Why would you use a Symbol?

153. Why are Symbols useful for unique object keys?

---

# 23. Iterators & Generators — Deep

For an architect-level interviewer, these are worth knowing.

154. What is an iterator?

155. What does an iterator need to implement?

```js
next()
```

156. What does `next()` return?

```js
{
  value: ...,
  done: ...
}
```

157. What is a generator?

```js
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}
```

158. Difference between `yield` and `return`.

159. Where could generators be useful?

---

# 24. Advanced Output Questions ⭐⭐⭐

These are especially good practice.

### Question 1

```js
var x = 1;

function foo() {
  console.log(x);
  var x = 2;
}

foo();
```

What happens?

---

### Question 2

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

Output?

Then:

> How would you make it print `0 1 2`?

---

### Question 3

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
```

Why is this different from `var`?

---

### Question 4

```js
console.log("start");

setTimeout(() => console.log("timeout"), 0);

Promise.resolve().then(() => console.log("promise"));

console.log("end");
```

Explain the exact execution order.

---

### Question 5

```js
const obj = {
  value: 10,

  getValue() {
    return this.value;
  }
};

const fn = obj.getValue;

console.log(fn());
```

Why?

---

### Question 6

```js
const obj = {
  value: 10,

  getValue: () => this.value
};

console.log(obj.getValue());
```

Why is this different?

---

# 25. Coding Questions You Should Be Able to Implement

For a 4+ year developer, I would practice these **without looking at Google**:

### Easy/Medium

* `map()`
* `filter()`
* `reduce()`
* `find()`
* `forEach()`
* `includes()`
* `debounce()`
* `throttle()`
* `deepClone()`
* `flattenArray()`
* `flattenObject()`
* `groupBy()`
* `uniqueArray()`

### Medium/Advanced

* `Promise.all()`
* `Promise.race()`
* `Promise.any()`
* Retry mechanism
* Concurrency limiter
* Memoization function
* Currying
* Partial application
* `Function.prototype.bind`
* Event emitter
* LRU cache
* Custom `map/filter/reduce`

---

# 26. Very Important: Memoization

Since we were discussing React's `useMemo`, your interviewer could connect it back to **JavaScript memoization**.

Question:

> "What is memoization?"

Example:

```js
function memoize(fn) {
  // implement
}

const expensive = memoize((n) => {
  console.log("calculating...");
  return n * n;
});

expensive(5);
expensive(5);
```

Expected:

```text
calculating...
25
25
```

The second call should use the cached result.

Then expect:

> "How would you handle multiple arguments?"

And:

> "What problems can an unlimited memoization cache cause?"

That's where you can discuss **memory growth / cache eviction**, which connects nicely to architecture.

---

# 27. JavaScript + React Questions

Because your background is React, the interviewer may deliberately connect JavaScript fundamentals to React.

### Question

Why does this cause unnecessary rendering?

```jsx
<Child user={{ name: "John" }} />
```

Answer involves:

```text
Object reference
       ↓
new object every render
       ↓
different reference
       ↓
props appear changed
```

---

### Another

Why might this cause a memoized child to render?

```jsx
<Child onClick={() => handleClick()} />
```

What happens with:

```jsx
<Child onClick={handleClick} />
```

This leads directly into:

```text
React.memo
useCallback
referential equality
```

---

# 28. The "Architect" Questions

These are the questions I'd especially prepare for your particular interviewer.

### Scenario 1

> Your Node API receives 1,000 requests/sec. One request performs a CPU-heavy calculation. What happens to other requests?

Think:

```text
Node event loop
       ↓
CPU-heavy synchronous code
       ↓
Event loop blocked
       ↓
other requests wait
```

Then discuss:

```text
Worker Threads
Child Processes
Separate services
Queues
Horizontal scaling
```

---

### Scenario 2

> You have 10 independent APIs to call. Would you use 10 `await`s sequentially?

Why/why not?

---

### Scenario 3

> A React page has 50,000 records and becomes slow. What would you investigate?

Expected discussion:

```text
Rendering
Filtering
Sorting
Memoization
Virtualization
Pagination
Network
Bundle size
```

---

### Scenario 4

> Your Node application memory keeps increasing. What could be happening?

Discuss:

```text
Memory leaks
Global references
Closures
Timers
Event listeners
Caches
Large objects
Database connections
```

---

### Scenario 5

> Why can Node handle many I/O requests despite JavaScript being single-threaded?

This is a **must-know** question for your transition.

You should be able to explain:

```text
JavaScript thread
      ↓
Event loop
      ↓
Non-blocking I/O
      ↓
OS / libuv
      ↓
Callback / Promise
      ↓
Event loop
```

---

# Your Priority List

Don't try to memorize all 150+ questions at once.

For **your interview**, I'd rank them:

### 🔴 Must know deeply

1. Scope
2. Hoisting
3. Closures
4. `this`
5. Event loop
6. Promises
7. Async/await
8. `call/apply/bind`
9. Prototypes
10. Objects & references
11. Shallow vs deep copy
12. Array methods
13. `map/filter/reduce`
14. Debounce/throttle
15. Event bubbling/capturing
16. Memory leaks
17. Map/Set
18. JavaScript + React rendering
19. Node event loop
20. Error handling

### 🟡 Know reasonably well

* Generators
* Iterators
* Symbols
* WeakMap/WeakSet
* Modules
* Currying
* Partial application
* IIFE
* Proxy/Reflect

### 🟢 Good to know

* Typed arrays
* ArrayBuffer
* Atomics
* SharedArrayBuffer
* Web Workers
* Worker Threads

---

## Most important advice for your interview

Because your interviewer is **architect-level**, don't answer questions only with definitions.

For example, don't say:

> "`useCallback` caches a function."

Instead, demonstrate the underlying JavaScript concept:

```text
Function created
      ↓
New function reference
      ↓
Props comparison
      ↓
React.memo
      ↓
Potential unnecessary child render
```

Likewise, don't just say:

> "Node is single-threaded."

Explain **what is single-threaded** and how **libuv, OS-level I/O, and the thread pool** allow Node to handle asynchronous work.

That's the level of reasoning that will make your transition from **React developer → full-stack developer** convincing.
