# Airbnb JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript*

> **Note**: this guide assumes you are using [Babel](https://babeljs.io), and requires that you use [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) or the equivalent. It also assumes you are installing shims/polyfills in your app, with [airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) or the equivalent.

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb-base.svg)](https://www.npmjs.com/package/eslint-config-airbnb-base)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

This guide is available in other languages too. See [Translation](#translation)

Other Style Guides

  - [ES5 (Deprecated)](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)
  - [React](react/)
  - [CSS-in-JavaScript](css-in-javascript/)
  - [CSS & Sass](https://github.com/airbnb/css)
  - [Ruby](https://github.com/airbnb/ruby)

## Table of Contents

  1. [类型](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Control Statements](#control-statements)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles)
  1. [Standard Library](#standard-library)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Chat With Us About JavaScript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)
  1. [Amendments](#amendments)

## 类型

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **基本类型**: 直接存取基本类型

    - `string` 字符串
    - `number` 数值
    - `boolean` 布尔类型
    - `null`
    - `undefined`
    - `symbol` 标识符

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - Symbols cannot be faithfully polyfilled, so they should not be used when targeting browsers/environments that don't support them natively.

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **复杂类型**: 通过引用的方式存取复杂类型

    - `object` 对象
    - `array` 数组
    - `function` 函数

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## 引用

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) 对所有的引用使用 `const`，避免使用 `var`。 eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > 为什么？这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

    ```javascript
    // 不应该使用
    var a = 1;
    var b = 2;

    // 推荐使用
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var)   如果你一定需要可变动的引用，使用 `let` 代替 `var`。 eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > 为什么？因为 let 是块级作用域，而 var 是函数作用域。

    ```javascript
    // 不应该使用
    var count = 1;
    if (true) {
      count += 1;
    }

    // 推荐使用, `let`.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope)注意 `let` 和 `const` 都是块级作用域。
  
    ```javascript
    // const and let only exist in the blocks they are defined in.
    // const 和 let 只存在于它们被定义的区块内。
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects对象

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new)  使用字面值创建对象. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)
 
    ```javascript
    // 不应该使用
    const item = new Object();

    // 推荐使用
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties)   创建有动态属性名的对象时，使用可被计算的属性名称。

    为什么?因为这样可以让你在一个地方定义所有的对象属性。

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // 不应该使用
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // 推荐使用
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand)   使用对象方法的简写。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    ```javascript
    // 不应该使用
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // 推荐使用
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) 使用对象属性值的简写。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)
  
    为什么？因为这样更短更有描述性。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // 不应该使用
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // 推荐使用
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) 在对象属性声明前把简写的属性分组。

    为什么？因为这样能清楚地看出哪些属性使用了简写。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // 不应该使用
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // 推荐使用
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) 只引用无效标识符的属性。 eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)
  
    为什么?一般来说，我们认为它更容易阅读。它改进了语法突出显示，并且更容易被许多JS引擎优化。

    ```javascript
    // 不应该使用
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // 推荐使用
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) 不调用`Object.prototype`的原型方法，如`hasOwnProperty`、`propertyis`可数和`isprototype`。

    为什么?这些方法可能会被问题对象的属性所影响——考虑hasOwnProperty:false——或者，客体可能是空对象（object.create（null））。

    ```javascript
    // 不应该使用
    console.log(object.hasOwnProperty(key));

    // 推荐使用
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // 更好的推荐
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    // ...
    console.log(has.call(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) 喜欢对象传播操作符而不是 [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 给浅拷贝对象。分配使用`object rest`运算符来获得一个带有某些属性的新对象。

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // 不应该使用
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // 推荐使用
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays数组

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) 使用字面值创建数组。 eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // 不应该使用
    const items = new Array();

    // 推荐使用
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) 向数组添加元素时使用 [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 替代直接赋值。

    ```javascript
    const someStack = [];

    // 不应该使用
    someStack[someStack.length] = 'abracadabra';

    // 推荐使用
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads)使用拓展运算符 `...` 复制数组。

    ```javascript
    // 不应该使用
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // 推荐使用
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a><a name="4.4"></a>
  - [4.4](#arrays--from) 把一个类数组对象转换成数组，使用扩展`...`，代替 [Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)。

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // 推荐使用
    const nodes = Array.from(foo);

    // 更好的写法
    const nodes = [...foo];
    ```

  <a name="arrays--mapping"></a>
  - [4.5](#arrays--mapping) 使用扩展`...`，代替 [Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 在映射iterable,因为它避免创建中间数组

    ```javascript
    // 不应该使用
    const baz = [...foo].map(bar);

    // 推荐使用
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.6](#arrays--callback-return)  在数组方法回调中使用return语句。如果函数体由一个单独的语句组成，返回一个没有副作用的表达式，那么就可以省略返回值。 following [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // 推荐使用
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // 推荐使用
    [1, 2, 3].map(x => x + 1);

    // 不应该使用 - 没有返回值意味着`acc`在第一次迭代之后就没有定义了
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
    });

    // 推荐使用
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
      return flatten;
    });

    // 不应该使用
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // 推荐使用
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.7](#arrays--bracket-newline) 如果一个数组有多个行，则在打开和关闭数组括号之前使用换行符。

    ```javascript
    // 不应该使用
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // 推荐使用
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring解构

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) 使用解构存取和使用多属性对象。eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring) jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

  为什么？因为解构能减少临时引用属性。

    ```javascript
    // 不应该使用
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // 推荐使用
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) 对数组使用解构赋值。 eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring) jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)
  
    ```javascript
    const arr = [1, 2, 3, 4];

    // 不应该使用
    const first = arr[0];
    const second = arr[1];

    // 推荐使用
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) 需要回传多个值时，使用对象解构，而不是数组解构。 jscs: [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)
  
    为什么？增加属性或者改变排序不会改变调用时的位置。

    ```javascript
    // 不应该使用
    function processInput(input) {
      // 然后一个奇迹发生
      return [left, right, top, bottom];
    }

    // 调用时需要考虑回调数据的顺序。
    const [left, __, top] = processInput(input);

    // 推荐使用
    function processInput(input) {
       // 然后一个奇迹发生
      return { left, right, top, bottom };
    }

    // 调用者只选择他们需要的数据
    const { left, top } = processInput(input);
    ```

**[⬆ back to top](#table-of-contents)**

## Strings字符串

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes)  字符串使用单引号 `''` 。 eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)
 

    ```javascript
    // 不应该使用
    const name = "Capt. Janeway";

    // 不应该使用 - 模板文字应该包含插值或换行符
    const name = `Capt. Janeway`;

    // 推荐使用
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) 字符串超过 100 个字节应该使用字符串连接号换行。过度使用字串连接符号可能会对性能造成影响
   为什么?断开的字符串是很痛苦的，并且使代码更难以搜索。

    ```javascript
    // 不应该使用
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // 不应该使用
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // 推荐使用
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals)  程序化生成字符串时，使用模板字符串代替字符串连接。 eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)
 
    为什么？模板字符串更为简洁，更具可读性。

    ```javascript
    // 不应该使用
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // 不应该使用
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // 不应该使用
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // 推荐使用
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) 不要在字符串上使用 `eval()` 它会打开太多的漏洞。 eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping)  不要不必要地转义字符串中的字符。 eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    为什么?反斜杠会损害可读性，因此只有在必要时才会出现。

    ```javascript
    // 不应该使用
    const foo = '\'this\' \i\s \"quoted\"';

    // 推荐使用
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ back to top](#table-of-contents)**

## Functions函数

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations)  使用命名函数表达式而不是函数声明。 eslint: [`func-style`](https://eslint.org/docs/rules/func-style) jscs: [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)
 
    为什么?函数声明被挂起，这意味着它很容易——太容易——在文件中定义函数之前引用该函数。这损害了可读性和可维护性。如果你发现一个函数的定义是大的或复杂的，它会干扰理解文件的其余部分，那么也许是时候把它提取到它自己的模块了！不要忘记显式地命名这个表达式，不管这个名称是从包含的变量中推断出来的（在现代浏览器中通常是这样的，或者在使用诸如babel这样的编译器时）。这消除了对错误调用堆栈的任何假设。([Discussion](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // 不应该使用
    function foo() {
      // ...
    }

    // 不应该使用
    const foo = function () {
      // ...
    };

    // 推荐使用
    // 词汇名称与可变引用调用（s）区分开来
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) 在括号中立即调用函数表达式。eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)
  
    为什么?一个立即被调用的函数表达式是一个单独的单元——包装它，它的调用parens，在parens中，干净地表达了这一点。请注意，在一个到处都是模块的世界里，你几乎不需要一个IIFE。

    ```javascript
    // immediately-invoked函数表达式(IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) 永远不要在一个非函数代码块 (`if`, `while`, etc)。中声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但它们的解析表现不一致。 eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)
  
  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **注意:** ECMA-262 把 `block` 定义为一组语句。函数声明不是语句。阅读 ECMA-262 关于这个问题的说明。

    ```javascript
    // 不应该使用
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // 推荐使用
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) 永远不要把参数命名为 `arguments`。 这将取代原来函数作用域内的`arguments`对象。

    ```javascript
    // 不应该使用
    function foo(name, options, arguments) {
      // ...
    }

    // 推荐使用
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) 不要使用 `arguments`。可以选择rest语法 `... `替代。eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)
  
    为什么？使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

    ```javascript
    // 不应该使用
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // 推荐使用
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

    ```javascript
    // 非常差的建议
    function handleThings(opts) {
      // 不！我们不应该改变函数参数。
      // 更加糟糕: 如果参数 opts 是 false 的话，它就会被设定为一个对象。
      // be what you want but it can introduce subtle bugs.（译注：例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象。）
      opts = opts || {};
      // ...
    }

    // 仍然差
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // 推荐使用
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) 直接给函数参数赋值时需要避免副作用。

    为什么？因为这样的写法让人感到很困惑。

    ```javascript
    var b = 1;
    // 不应该使用
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) 总是把默认参数放在最后。

    ```javascript
    // 不应该使用
    function handleThings(opts = {}, name) {
      // ...
    }

    // 推荐使用
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor)千万不要使用函数构造函数来创建一个新函数。 eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)
  
    为什么?以这种方式创建一个函数来评估一个类似于eval（）的字符串，这会打开漏洞。

    ```javascript
    // 不应该使用
    var add = new Function('a', 'b', 'return a + b');

    // 仍旧不推荐
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing)   在函数签名中间隔。eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    为什么?一致性是好的，你不应该在添加或删除名称时添加或删除空格。

    ```javascript
    // 不应该使用
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // 推荐使用
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params)  没有变异参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    为什么?作为参数传递进来的物体会在原始调用者中引起不必要的可变副作用。

    ```javascript
    // 不应该使用
    function f1(obj) {
      obj.key = 1;
    }

    // 推荐使用
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params)  永远不会再分配参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    为什么?重新分配参数会导致意外的行为，特别是在访问参数对象时。它还可能导致优化问题，尤其是在V8中。

    ```javascript
    // 不应该使用
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // 推荐使用
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) 更喜欢使用传播操作符 `...` 调用可变函数。 eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    为什么?它更干净，你不需要提供上下文，而且你不能很容易地用应用来组合新的内容。

    ```javascript
    // 不应该使用
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // 推荐使用
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // 不应该使用
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // 推荐使用
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, with a trailing comma on the last item. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)
  具有多行签名或调用的函数应该像本指南中的每一个多行列表一样缩进：每一项都在一行上，在最后一项上有一个末尾逗号。

    ```javascript
    // 不应该使用
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // 推荐使用
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // 不应该使用
    console.log(foo,
      bar,
      baz);

    // 推荐使用
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ back to top](#table-of-contents)**

## Arrow Functions 箭头函数

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。 eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)
  
   为什么？因为箭头函数创造了新的一个 this 执行环境（译注：参考 Arrow functions - JavaScript | MDN 和 ES6 arrow functions, syntax and lexical scoping），通常情况下都能满足你的需求，而且这样的写法更为简洁。

   为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

    ```javascript
    // 不应该使用
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // 推荐使用
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) 如果函数体由一个单独的语句组成，返回一个没有副作用的表达式（[expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)），那么省略括号并使用隐式返回。否则，保留括号并使用return语句。  eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)
  
   为什么?语法糖。在链式调用中可读性很高。

    ```javascript
    // 不应该使用
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // 推荐使用
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // 推荐使用
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // 推荐使用
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // 无隐式返回和副作用
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // 如果回调返回true的话
      }
    }

    let bool = false;

    // 不应该使用
    foo(() => bool = true);

    // 推荐使用
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) 如果表达式跨越多个行，将其封装在括号中以提高可读性。

   为什么?它清楚地显示了函数的开始和结束。

    ```javascript
    // 不应该使用
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // 推荐使用
    ['get', 'post', 'put'].map(httpMethod => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) 如果您的函数只接受一个参数，并且不使用括号，那么省略括号。否则，总是在参数周围加上括号，以保持清晰和一致性。注意：始终使用括号也是可以接受的，在这种情况下，使用[“always” option](https://eslint.org/docs/rules/arrow-parens#always)[`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam) 的eslint选项，或者不包括用于jscs的不允许的括号。  eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

    为什么?少了视觉上的混乱。

    ```javascript
    // 不应该使用
    [1, 2, 3].map((x) => x * x);

    // 推荐使用
    [1, 2, 3].map(x => x * x);

    // 推荐使用
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // 不应该使用
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // 推荐使用
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) 避免混淆箭头函数语法 (`=>`) 和比较运算符 (`<=`, `>=`)。 eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // 不应该使用
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // 不应该使用
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // 推荐使用
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // 推荐使用
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) 总是使用 `class`。避免直接操作 `prototype` 。

    为什么? 因为 `class` 语法更为简洁更易读。

    ```javascript
    // 不应该使用
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // 推荐使用
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) 使用 `extends` 继承。

    为什么？因为 `extends` 是一个内建的原型继承方法并且不会破坏 `instanceof`。

    ```javascript
    // 不应该使用
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // 推荐使用
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) 方法可以返回 `this` 来帮助链式调用。

    ```javascript
    // 不应该使用
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // 推荐使用
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"> 可以写一个自定义的 toString() 方法，但要确保它能正常运行并且不会引起副作用。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 如果没有指定的话，类就有一个默认的构造函数。一个空的构造函数或者仅仅是委托给父类的函数是没有必要的。
 eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)
  
    ```javascript
    // 不应该使用
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // 不应该使用
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // 推荐使用
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) 避免重复的类成员。 eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)
  
    为什么?重复的类成员声明将会悄悄地选择最后一个——拥有副本几乎肯定是一个bug。

    ```javascript
    // 不应该使用
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // 推荐使用
    class Foo {
      bar() { return 1; }
    }

    // 推荐使用
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Modules 模块

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) 总是使用模组 (`import`/`export`) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。

    为什么？模块就是未来，让我们开始迈向未来吧。

    ```javascript
    // 不应该使用
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) 不要使用通配符 import。

    为什么？这样能确保你只有一个默认 export。

    ```javascript
    // 不应该使用
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // 推荐使用
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) 不要从 import 中直接 export。

   为什么？虽然一行代码简洁明了，但让 import 和 export 各司其职让事情能保持一致。

    ```javascript
    // 不应该使用
    // filename es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // 推荐使用
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports)  只从一个地方的路径导入。
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
 
    为什么?从相同的路径导入多条线路会使代码更难维护。

    ```javascript
    // 不应该使用
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // 推荐使用
    import foo, { named1, named2 } from 'foo';

    // 推荐使用
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports)   不要导出可变绑定。
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)


    为什么?一般来说，应该避免突变，特别是在导出可变绑定时。虽然在某些特殊情况下可能需要这种技术，但一般来说，只有经常引用才能导出。

    ```javascript
    // 不应该使用
    let foo = 3;
    export { foo };

    // 推荐使用
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export)   在具有单个导出的模块中，首选缺省出口优于命名出口。
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)


    为什么?为了鼓励更多的文件只导出一件事，这对可读性和可维护性更好。

    ```javascript
    // 不应该使用
    export function foo() {}

    // 推荐使用
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) 把所有的`import`放在非import声明的上面
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    
    为什么?由于`import`是被悬挂的，所以把它们放在顶部可以防止意外的行为。

    ```javascript
    // 不应该使用
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // 推荐使用
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines)  多行导入应该像多行数组和对象文字一样缩进。

    为什么?花括号与样式指南中的其他花括号一样，都遵循相同的缩进规则，后面的逗号也是如此。

    ```javascript
    // 不应该使用
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // 推荐使用
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax)  在模块导入语句中不允许Webpack装载器语法。
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

    为什么?因为在导入中使用Webpack语法将代码变成了模块bundler。更喜欢在`webpack.config.js`中使用装载机语法。

    ```javascript
    // 不应该使用
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // 推荐使用
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) 不要使用iterators。更喜欢JavaScript的高阶函数，而不是像`for-in` or `for-of`那样的循环。 eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    为什么?这将强制我们不可变规则。处理返回值的纯函数比副作用更容易推理。

    > Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... to iterate over arrays, and `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // 不应该使用
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // 推荐使用
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // 更好的建议 (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // 不应该使用
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // 推荐使用
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // 更好的建议 (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) 现在还不要使用 generators。

     为什么？因为它们现在还没法很好地编译到 ES5。

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) 如果您必须使用generators，或者您不理会[我们的建议](#generators--nope)，请确保它们的功能签名是正确的。 eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)
  

    为什么?`function`和`*`是同一个概念关键字的一部分——`*`不是函数`function`的修饰符，`function*`是唯一的构造，不同于`function`。

    ```javascript
    // 不应该使用
    function * foo() {
      // ...
    }

    // 不应该使用
    const bar = function * () {
      // ...
    };

    // 不应该使用
    const baz = function *() {
      // ...
    };

    // 不应该使用
    const quux = function*() {
      // ...
    };

    // 不应该使用
    function*foo() {
      // ...
    }

    // 不应该使用
    function *foo() {
      // ...
    }

    // 更不应该使用
    function
    *
    foo() {
      // ...
    }

    // 更不应该使用
    const wat = function
    *
    () {
      // ...
    };

    // 推荐使用
    function* foo() {
      // ...
    }

    // 推荐使用
    const foo = function* () {
      // ...
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Properties 属性

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) 在访问属性时使用点符号。 eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // 不应该使用
    const isJedi = luke['jedi'];

    // 推荐使用
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) 当通过变量访问属性时使用中括号 `[]`。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```
  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator)  在计算指数时使用`**`。eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).
 
    ```javascript
    // 不应该使用
    const binary = Math.pow(2, 10);

    // 推荐使用
    const binary = 2 ** 10;
    ```

**[⬆ back to top](#table-of-contents)**

## Variables 变量

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) 一直使用 `const`或`let` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。地球队长已经警告过我们了。（译注：全局，global 亦有全球的意思。地球队长的责任是保卫地球环境，所以他警告我们不要造成「全球」污染。） eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)
  

    ```javascript
    // 不应该使用
    superPower = new SuperPower();

    // 推荐使用
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const)   使用 `const`或`let` 声明每一个变量。eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)


    为什么?用这种方式添加新的变量声明更容易，而且您永远不必担心交换a`;`对于a`，`或者引入标点的问题。您还可以使用调试器逐步完成每个声明，而不是一次性跳过所有的声明。

    ```javascript
    // 不应该使用
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // 不应该使用
    // (与上面比较，试着找出错误)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // 推荐使用
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) 将所有的 `const` 和 `let` 分组

    为什么？当你需要把已赋值变量赋值给未赋值变量时非常有用。

    ```javascript
    // 不应该使用
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // 不应该使用
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // 推荐使用
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) A在需要的地方分配变量，但是把它们放在一个合理的地方。

    为什么?让`let`和`const`块作用域,而不是函数作用域。

    ```javascript
    // 不应该使用 - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // 推荐使用
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment)   不变量作业链。 eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    为什么?链接变量赋值会创建隐式全局变量。

    ```javascript
    // 不应该使用
    (function example() {
      // JavaScript解释这个
      // let a = ( b = ( c = 1 ) );
      // let关键字只适用于变量a;变量b和c变成
      // 全局变量.
      let a = b = c = 1;
    }());

    console.log(a); // 抛出ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // 推荐使用
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // 抛出ReferenceError
    console.log(b); // 抛出ReferenceError
    console.log(c); // 抛出ReferenceError

    // 这同样适用于“const”
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement)避免使用一元增量和递减（++，-）。eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)
  

    为什么?在eslint文档中，一元递增和递减语句被自动分号插入，并且会导致在应用程序中增加或递减值的无声错误。它也更能表达你的价值观，比如`num+=1`，而不是`num++`或`num++`。不允许非ary的增量和递减语句也会阻止你在无意中预先增加/预减值，这也会在你的程序中引起意想不到的行为。

    ```javascript
    // 不应该使用

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // 推荐使用

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>
  - [13.7](#variables--linebreak) 避免在 `= `之前或之后有换行。如果你的作业违反了 [`max-len`](https://eslint.org/docs/rules/max-len.html), 将值包围在parens中。 eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    为什么?线框的周围`=`可以混淆任务的值。

    ```javascript
    // 不应该使用
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // 不应该使用
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // 推荐使用
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // 推荐使用
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var` 声明会被提升至该作用域的顶部，但它们赋值不会提升。`let` 和 `const` 被赋予了一种称为[Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let)的概念。这对于了解为什么 [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)相当重要。

    ```javascript
    ///我们知道这是行不通的（假设有的话是没有定义的全局变量）
    function example() {
      console.log(notDefined); // => 抛出一个ReferenceError
    }

    
    // 由于变量提升的原因，
    // 在引用变量后再声明变量是可以运行的。
    // 注：变量的赋值 `true` 不会被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 编译器会把函数声明提升到作用域的顶层，
    // 这意味着我们的例子可以改写成这样：
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // 使用 const 和 let
    function example() {
      console.log(declaredButNotAssigned); // => 抛出一个ReferenceError
      console.log(typeof declaredButNotAssigned); // => 抛出一个ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) 匿名函数表达式的变量名会被提升，但函数内容并不会。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => 类型错误匿名不是一个函数

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => 类型错误命名不是一个函数

      superPower(); // => 参考错误superPower没有定义

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 函数名也是如此与变量名相同。
    function example() {
      console.log(named); // => undefined

      named(); // => 类型错误命名不是一个函数

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) 函数声明的名称和函数体都会被提升。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 更多信息请参阅 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) 像`if`语句这样的条件语句使用`ToBoolean`抽象方法来评估它们的表达式，并且总是遵循这些简单的规则：

    - **Objects** 被计算为 **true**
    - **Undefined** 被计算为 **false**
    - **Null** 被计算为 **false**
    - **Booleans** 被计算为 **布尔的值**
    - **Numbers** 被计算为 **false** 如果是 **+0, -0, or NaN**, 否则为 **true**
    - **Strings** 被计算为 **false** 如果是空字符串 `''`, 否则为 **true**

    ```javascript
    if ([0] && []) {
      // true
      // 一个数组（甚至是空的）是一个对象，对象将被评估为true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) 对布尔值使用快捷方式，但对字符串和数字进行显式比较。

    ```javascript
    // 不应该使用
    if (isValid === true) {
      // ...
    }

    // 推荐使用
    if (isValid) {
      // ...
    }

    // 不应该使用
    if (name) {
      // ...
    }

    // 推荐使用
    if (name !== '') {
      // ...
    }

    // 不应该使用
    if (collection.length) {
      // ...
    }

    // 推荐使用
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo)  想了解更多信息，参考 Angus Croll 的 [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) 使用括号来创建包含词法声明的`case`和`default`（例如`let`、`const`、`function`和`class`）。eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)


    为什么?词法声明在整个`switch`块中是可见的，但是只有在分配时才会被初始化，只有当它的`case` 达到时才会发生。当多个`case`子句试图定义相同的东西时，这会导致问题。

    ```javascript
    // 不应该使用
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // 推荐使用
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Ternaries不应该被嵌套，通常是单行表达式。 eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)
  
    ```javascript
    // 不应该使用
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // 分成2个分离的三元表达式
    const maybeNull = value1 > value2 ? 'baz' : null;

    // 比较好的推荐
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // 更好的推荐
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) 避免不必要的三元语句。eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // 不应该使用
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // 推荐使用
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) 在混合操作符时，将它们括在括号内。唯一的例外是标准算术运算符 (`+`, `-`, `*`, & `/`) 因为它们的优先级被广泛理解。 eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    为什么?这提高了可读性，并澄清了开发人员的意图。

    ```javascript
    // 不应该使用
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // 不应该使用
    const bar = a ** b - 5 % d;

    // 不应该使用
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d;
    }

    // 推荐使用
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // 推荐使用
    const bar = (a ** b) - (5 % d);

    // 推荐使用
    if (a || (b && c)) {
      return d;
    }

    // 推荐使用
    const bar = a + b / c * d;
    ```

**[⬆ back to top](#table-of-contents)**

## Blocks 代码块

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces)  使用大括号包裹所有的多行代码块。 eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // 不应该使用
    if (test)
      return false;

    // 推荐使用
    if (test) return false;

    // 推荐使用
    if (test) {
      return false;
    }

    // 不应该使用
    function foo() { return false; }

    // 推荐使用
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。 eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

    ```javascript
    // 不应该使用
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // 推荐使用
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) 如果`If`块总是执行`return`语句，那么后面的`else`块就没有必要了。如果块中包含`return`的块块后面的块可以被分隔成多个`if`块。 eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)


    ```javascript
    // 不应该使用
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // 不应该使用
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // 不应该使用
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // 推荐使用
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // 推荐使用
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // 推荐使用
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Control Statements 

  <a name="control-statements"></a>
  - [17.1](#control-statements) 如果您的控制语句（`if`，`while`等等）太长或超过最大行长度，则可以将每个（grouped）条件放入新行中。逻辑运算符应该开始这一行。

    为什么?在一行的开头要求操作符使操作符保持一致，并遵循类似于方法链接的模式。这也提高了可读性，使其更易于直观地遵循复杂的逻辑。

    ```javascript
    // 不应该使用
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // 不应该使用
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // 不应该使用
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // 不应该使用
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // 推荐使用
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // 推荐使用
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // 推荐使用
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a>
  - [17.2](#control-statements--value-selection) 不要使用选择操作符来代替控制语句。

    ```javascript
    // 不应该使用
    !isRunning && startRunning();

    // 推荐使用
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments 注释

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // 不应该使用
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // 推荐使用
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

    ```javascript
    // 不应该使用
    const active = true;  // is current tab

    // 推荐使用
    // is current tab
    const active = true;

    // 不应该使用
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // 推荐使用
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) 用一个空格开始所有的注释，使它更容易阅读。 eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)
 

    ```javascript
    // 不应该使用
    //is current tab
    const active = true;

    // 推荐使用
    // is current tab
    const active = true;

    // 不应该使用
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // 推荐使用
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems)  给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Use `// FIXME:` to annotate problems.
  使用 // FIXME: 标注问题。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) 使用 `// TODO:` 标注问题的解决方式。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace 空白

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) 使用 2 个空格作为缩进。 eslint: [`indent`](https://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)
  

    ```javascript
    // 不应该使用
    function foo() {
    ∙∙∙∙let name;
    }

    // 不应该使用
    function bar() {
    ∙let name;
    }

    // 推荐使用
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks)  在花括号前放一个空格。 eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)


    ```javascript
    // 不应该使用
    function test(){
      console.log('test');
    }

    // 推荐使用
    function test() {
      console.log('test');
    }

    // 不应该使用
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // 推荐使用
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。 eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)
  

    ```javascript
    // 不应该使用
    if(isJedi) {
      fight ();
    }

    // 推荐使用
    if (isJedi) {
      fight();
    }

    // 不应该使用
    function fight () {
      console.log ('Swooosh!');
    }

    // 推荐使用
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) 使用空格把运算符隔开。 eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)
  

    ```javascript
    // 不应该使用
    const x=y+5;

    // 推荐使用
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end)  在文件末尾插入一个空行。 eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)


    ```javascript
    // 不应该使用
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // 不应该使用
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // 推荐使用
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains)  在使用长方法链时进行缩进。使用前面的点 . 强调这是方法调用而不是新语句。 eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)
   


    ```javascript
    // 不应该使用
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // 不应该使用
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // 推荐使用
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // 不应该使用
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // 推荐使用
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // 推荐使用
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) 在块末和新语句前插入空行。 jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)
 

    ```javascript
    // 不应该使用
    if (foo) {
      return bar;
    }
    return baz;

    // 推荐使用
    if (foo) {
      return bar;
    }

    return baz;

    // 不应该使用
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // 推荐使用
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // 不应该使用
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // 推荐使用
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks)   块里面不要有多余的换行符。eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)


    ```javascript
    // 不应该使用
    function bar() {

      console.log(foo);

    }

    // 不应该使用
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // 不应该使用
    class Foo {

      constructor(bar) {
        this.bar = bar;
      }
    }

    // 推荐使用
    function bar() {
      console.log(foo);
    }

    // 推荐使用
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.9](#whitespace--in-parens)  不要在括号中添加空格。 eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)
 

    ```javascript
    // 不应该使用
    function bar( foo ) {
      return foo;
    }

    // 推荐使用
    function bar(foo) {
      return foo;
    }

    // 不应该使用
    if ( foo ) {
      console.log(foo);
    }

    // 推荐使用
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.10](#whitespace--in-brackets) 不要在括号内添加空格。 eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)


    ```javascript
    // 不应该使用
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // 推荐使用
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.11](#whitespace--in-braces)   在花括号中添加空格。eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)


    ```javascript
    // 不应该使用
    const foo = {clark: 'kent'};

    // 推荐使用
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.12](#whitespace--max-len) 避免使用长度超过100个字符的代码行（包括空格）。 注意：在[above](#strings--line-length), 长字符串不受此规则的限制，不应该被分解。 eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

    为什么?这确保了可读性和可维护性。

    ```javascript
    // 不应该使用
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // 不应该使用
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // 推荐使用
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // 推荐使用
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ back to top](#table-of-contents)**

## Commas 逗号

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) 行首逗号： **不需要。** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)
  

    ```javascript
    // 不应该使用
    const story = [
        once
      , upon
      , aTime
    ];

    // 推荐使用
    const story = [
      once,
      upon,
      aTime,
    ];

    // 不应该使用
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // 推荐使用
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling)  增加结尾的逗号: **需要。** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)
  

    为什么? 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的[尾逗号问题](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers。

    ```diff
    // 不应该使用 - 不带逗号的git diff
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // 推荐使用 - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // 不应该使用
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // 推荐使用
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // 不应该使用
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // 推荐使用
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // 推荐使用 (注意，逗号不能在“rest”元素之后出现)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // 不应该使用
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // 推荐使用
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // 推荐使用 (注意，逗号不能在“rest”元素之后出现)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons 分号

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **使用分号。** eslint: [`semi`](https://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)
  

    为什么?当JavaScript遇到换行符没有分号,它使用的一组规则称为自动分号来确定是否应将换行符 [插入](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)结束的一份声明中,和(顾名思义)分号之前到你的代码换行是否这么认为。不过，ASI包含了一些古怪的行为，如果JavaScript误解了您的换行符，那么您的代码就会中断。随着新特性成为JavaScript的一部分，这些规则将变得更加复杂。显式地终止您的语句并配置您的linter以捕获缺失

    ```javascript
    // 不应该使用 - raises exception
    const luke = {}
    const leia = {}
    [luke, leia].forEach(jedi => jedi.father = 'vader')

    // 不应该使用 - raises exception
    const reaction = "No! That's impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // 不应该使用 - 返回`undefined`，而不是下一行的值——总是发生在`return`本身因为ASI而在一行上
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // 推荐使用
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // 推荐使用
    const reaction = "No! That's impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // 推荐使用
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion 类型转换

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) 在语句开始时执行类型转换。

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings)  字符串： eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)
  
    ```javascript
    // => this.reviewScore = 9;

    // 不应该使用
    const totalScore = new String(this.reviewScore); // totalScore的类型是“对象”而不是“字符串”

    // 不应该使用
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // 不应该使用
    const totalScore = this.reviewScore.toString(); // 不能保证返回一个字符串

    // 推荐使用
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) Numbers: 对`Number`使用 `parseInt` 转换，并带上类型转换的基数。eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)
  

    ```javascript
    const inputValue = '4';

    // 不应该使用
    const val = new Number(inputValue);

    // 不应该使用
    const val = +inputValue;

    // 不应该使用
    const val = inputValue >> 0;

    // 不应该使用
    const val = parseInt(inputValue);

    // 推荐使用
    const val = Number(inputValue);

    // 推荐使用
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations)  如果因为某些原因 `parseInt` 成为你所做的事的瓶颈而需要使用位操作解决 [性能问题时](https://jsperf.com/coercion-vs-casting/3)，留个注释说清楚原因和你的目的。

    ```javascript
    // 推荐使用
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **注意：** 小心使用位操作运算符。数字会被当成[ 64 位值](https://es5.github.io/#x4.3.19)，但是位操作运算符总是返回 32 位的整数 ([参考](https://es5.github.io/#x11.7))。位操作处理大于 32 位的整数值时还会导致意料之外的行为。关于这个问题的[讨论](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整数是 2,147,483,647：

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) 布尔: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)
  

    ```javascript
    const age = 0;

    // 不应该使用
    const hasAge = new Boolean(age);

    // 推荐使用
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions 命名规则

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) 避免单字母命名。命名应具备描述性。eslint: [`id-length`](https://eslint.org/docs/rules/id-length)
 

    ```javascript
    // 不应该使用
    function q() {
      // ...
    }

    // 推荐使用
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase)  使用驼峰式命名对象、函数和实例。 eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)
 

    ```javascript
    // 不应该使用
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // 推荐使用
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase)  使用帕斯卡式命名构造函数或类。 eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)


    ```javascript
    // 不应该使用
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // 推荐使用
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) 不要使用下划线 _ 结尾或开头来命名属性和方法。eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)
  

    为什么?在属性或方法方面，JavaScript没有隐私的概念。尽管一个主要的下划线是一个普通的约定，意思是“私有”，实际上，这些属性是完全公开的，因此，是您的公共API契约的一部分。这个约定可能会导致开发人员错误地认为变更不会被认为是破坏，或者不需要测试。如果你想要一些“私人”的东西，那它就不能被观察到。

    ```javascript
    // 不应该使用
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // 推荐使用
    this.firstName = 'Panda';

    // 推荐使用, 在可用弱映射的环境中
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this)  别保存 `this` 的引用。使用箭头函数或[Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

    ```javascript
    // 不应该使用
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // 不应该使用
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // 推荐使用
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // 不应该使用
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // 不应该使用
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // 推荐使用
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [23.7](#naming--camelCase-default-export) 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) 当你导出 constructor / class / singleton / function library / bare object时使用帕斯卡式命名。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) 缩略词和初始化应该总是大写的，或者所有小写的。

    为什么?名称是为了便于阅读，而不是为了满足计算机算法。

    ```javascript
    // 不应该使用
    import SmsContainer from './containers/SmsContainer';

    // 不应该使用
    const HttpRequests = [
      // ...
    ];

    // 推荐使用
    import SMSContainer from './containers/SMSContainer';

    // 推荐使用
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) 您可以选择大写一个常量，只有当它（1）被导出时，（2）是一个`const`（它不能被重新分配），并且（3）程序员可以信任它（及其嵌套的属性）永远不会改变。

    为什么?这是一个额外的工具，可以帮助程序员不确定变量是否会发生变化。上案例变量让程序员知道他们可以信任变量（及其属性）不改变。
    那么所有的`const`变量呢？-这是不必要的，所以在一个文件中不应该使用上框。然而，它应该用于导出的常量。
    导出的对象呢?-在出口的顶层（如`EXPORTED_OBJECT.key`）上的大写，并保持所有嵌套属性都不会改变。

    ```javascript
    // 不应该使用
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // 不应该使用
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // 不应该使用
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // 不应该使用 - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    };

    // 推荐使用
    export const MAPPING = {
      key: 'value'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Accessors 存取器

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) 属性的存取函数不是必须的。

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) 不要使用JavaScript getter/setter，因为它们会导致意想不到的副作用，而且更难测试、维护和推理。相反，如果您确实制作了存取器函数，请使用getVal（）和setVal（'hello'）。

    ```javascript
    // 不应该使用
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // 推荐使用
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) 如果属性是`布尔值`，使用 isVal() 或 hasVal()。

    ```javascript
    // 不应该使用
    if (!dragon.age()) {
      return false;
    }

    // 推荐使用
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [24.4](#accessors--consistent) 创建 get() 和 set() 函数是可以的，但要保持一致。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Events 事件

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) 当给事件附加数据时（无论是 DOM 事件还是私有事件），传入一个哈希而不是原始值。这样可以让后面的贡献者增加更多数据到事件数据而无需找出并更新事件的每一个处理器。例如，不好的写法：

    ```javascript
    // 不应该使用
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    });
    ```

    prefer:

    ```javascript
    // 推荐使用
    $(this).trigger('listingUpdated', { listingID: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    });
    ```

  **[⬆ back to top](#table-of-contents)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [26.1](#jquery--dollar-prefix) 使用 `$` 作为存储 jQuery 对象的变量名前缀。 jscs: [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)
  
    ```javascript
    // 不应该使用
    const sidebar = $('.sidebar');

    // 推荐使用
    const $sidebar = $('.sidebar');

    // 推荐使用
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [26.2](#jquery--cache) 缓存 jQuery 查询。

    ```javascript
    // 不应该使用
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // 推荐使用
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [26.3](#jquery--queries) 对 DOM 查询使用层叠 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [26.4](#jquery--find) 对有作用域的 jQuery 对象查询使用 `find`。

    ```javascript
    // 不应该使用
    $('ul', '.sidebar').hide();

    // 不应该使用
    $('.sidebar').find('ul').hide();

    // 推荐使用
    $('.sidebar ul').hide();

    // 推荐使用
    $('.sidebar > ul').hide();

    // 推荐使用
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

## ECMAScript 5 Compatibility ECMAScript 5 兼容性

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [27.1](#es5-compat--kangax) 参考 [Kangax](https://twitter.com/kangax/)’s ES5 [兼容性](https://kangax.github.io/es5-compat-table/)。

**[⬆ back to top](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6+ (ES 2015+) Styles
ECMAScript 6 规范

  <a name="es6-styles"></a><a name="27.1"></a>
  - [28.1](#es6-styles) 以下是链接到 ES6 各个特性的列表。

1. [Arrow Functions](#arrow-functions)箭头函数
1. [Classes](#classes--constructors)类
1. [Object Shorthand](#es6-object-shorthand)对象方法简写
1. [Object Concise](#es6-object-concise)对象属性简写
1. [Object Computed Properties](#es6-computed-properties)对象中的可计算属性
1. [Template Strings](#es6-template-literals)模板字符串
1. [Destructuring](#destructuring)解构
1. [Default Parameters](#es6-default-parameters)默认参数
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)指数运算符
1. [Iterators and Generators](#iterators-and-generators)迭代器和生成器
1. [Modules](#modules)模块

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) 不要使用未达到第三阶段的[TC39建议](https://github.com/tc39/proposals)。
  

    > 为什么? [它们还没有最终确定](https://tc39.github.io/process-document/), 它们可能会发生变化，或者完全退出。我们想要使用JavaScript，而建议还不是JavaScript。

**[⬆ back to top](#table-of-contents)**

## Standard Library 标准程序库

  The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
  contains utilities that are functionally broken but remain for legacy reasons.
  标准库包含功能中断的实用程序，但由于遗留原因而保留。

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan)  使用`Number.isNaN`，而不是全局`isNaN`。
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)
   
    为什么?全局的`isNaN`强迫非数字的数字，对于任何强迫NaN的东西都是true。如果这种行为是需要的，那么就把它显式。

    ```javascript
    // 不应该使用
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // 推荐使用
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) 使用`Number.isFinite`替代全局`isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    为什么?全局是`isFinite`，非数字的数字，对于任何强迫到有限数字的东西都是true。如果这种行为是需要的，那么就把它显式。

    ```javascript
    // 不应该使用
    isFinite('2e3'); // true

    // 推荐使用
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ back to top](#table-of-contents)**

## Testing

  <a name="testing--yup"></a><a name="28.1"></a>
  - [30.1](#testing--yup) **Yup.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.2](#testing--for-real) **没有,但是认真**:
    无论您使用哪种测试框架，都应该编写测试！
    努力写出许多小的纯函数，并最小化发生突变的地方。
    对存根和mock保持谨慎——它们可以使您的测试更加脆弱。
    我们主要在Airbnb上使用 [`mocha`](https://www.npmjs.com/package/mocha)和笑话。[`tape`](https://www.npmjs.com/package/tape)有时也用于小的、独立的模块。
    百分百的测试覆盖率是一个很好的目标，即使它并不总是可行的。
    当您修复一个bug时，编写一个回归测试。一个没有回归测试的bug几乎肯定会在将来再次崩溃。

**[⬆ back to top](#table-of-contents)**

## Performance

  - [On Layout & Web Performance](https://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](https://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](https://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](https://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](https://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](https://jsperf.com/ya-string-concat)
  - [Are Javascript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - Loading...

**[⬆ back to top](#table-of-contents)**

## Resources

**Learning ES6+**

  - [Latest ECMA spec](https://tc39.github.io/ecma262/)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  - Code Style Linters
    - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    - [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    - [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json) (Deprecated, please use [ESlint](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base))
  - Neutrino preset - [neutrino-preset-airbnb-base](https://neutrino.js.org/presets/neutrino-preset-airbnb-base/)

**Other Style Guides**

  - [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
  - [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don’t Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](https://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Air](https://javascriptair.com/)
  - [JavaScript Jabber](https://devchat.tv/js-jabber/)

**[⬆ back to top](#table-of-contents)**

## In the Wild

  This is a list of organizations that are using this style guide. Send us a pull request and we'll add you to the list.

  - **123erfasst**: [123erfasst/javascript](https://github.com/123erfasst/javascript)
  - **3blades**: [3Blades](https://github.com/3blades)
  - **4Catalyzer**: [4Catalyzer/javascript](https://github.com/4Catalyzer/javascript)
  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **AltSchool**: [AltSchool/javascript](https://github.com/AltSchool/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Ascribe**: [ascribe/javascript](https://github.com/ascribe/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Avant**: [avantcredit/javascript](https://github.com/avantcredit/javascript)
  - **Axept**: [axept/javascript](https://github.com/axept/javascript)
  - **BashPros**: [BashPros/javascript](https://github.com/BashPros/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk](https://github.com/Bisk/)
  - **Bonhomme**: [bonhommeparis/javascript](https://github.com/bonhommeparis/javascript)
  - **Brainshark**: [brainshark/javascript](https://github.com/brainshark/javascript)
  - **CaseNine**: [CaseNine/javascript](https://github.com/CaseNine/javascript)
  - **Chartboost**: [ChartBoost/javascript-style-guide](https://github.com/ChartBoost/javascript-style-guide)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **DoSomething**: [DoSomething/eslint-config](https://github.com/DoSomething/eslint-config)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **EvozonJs**: [evozonjs/javascript](https://github.com/evozonjs/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia](https://github.com/gawkermedia/)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **Generation Tux**: [GenerationTux/javascript](https://github.com/generationtux/styleguide)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **GreenChef**: [greenchef/javascript](https://github.com/greenchef/javascript)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **Grupo-Abraxas**: [Grupo-Abraxas/javascript](https://github.com/Grupo-Abraxas/javascript)
  - **Honey**: [honeyscience/javascript](https://github.com/honeyscience/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **Huballin**: [huballin](https://github.com/huballin/)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InterCity Group**: [intercitygroup/javascript-style-guide](https://github.com/intercitygroup/javascript-style-guide)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JeopardyBot**: [kesne/jeopardy-bot](https://github.com/kesne/jeopardy-bot/blob/master/STYLEGUIDE.md)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kaplan Komputing**: [kaplankomputing/javascript](https://github.com/kaplankomputing/javascript)
  - **KickorStick**: [kickorstick](https://github.com/kickorstick/)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **LEINWAND**: [LEINWAND/javascript](https://github.com/LEINWAND/javascript)
  - **Lonely Planet**: [lonelyplanet/javascript](https://github.com/lonelyplanet/javascript)
  - **M2GEN**: [M2GEN/javascript](https://github.com/M2GEN/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber](https://github.com/muber/)
  - **National Geographic**: [natgeo](https://github.com/natgeo/)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nulogy**: [nulogy/javascript](https://github.com/nulogy/javascript)
  - **Orange Hill Development**: [orangehill/javascript](https://github.com/orangehill/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **OutBoxSoft**: [OutBoxSoft/javascript](https://github.com/OutBoxSoft/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **React**: [facebook.github.io/react/contributing/how-to-contribute.html#style-guide](https://facebook.github.io/react/contributing/how-to-contribute.html#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/rei/code-style-guides/)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **Sainsbury's Supermarkets**: [jsainsburyplc](https://github.com/jsainsburyplc)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Sourcetoad**: [sourcetoad/javascript](https://github.com/sourcetoad/javascript)
  - **Springload**: [springload](https://github.com/springload/)
  - **StratoDem Analytics**: [stratodem/javascript](https://github.com/stratodem/javascript)
  - **SteelKiwi Development**: [steelkiwi/javascript](https://github.com/steelkiwi/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **SwoopApp**: [swoopapp/javascript](https://github.com/swoopapp/javascript)
  - **SysGarage**: [sysgarage/javascript-style-guide](https://github.com/sysgarage/javascript-style-guide)
  - **Syzygy Warsaw**: [syzygypl/javascript](https://github.com/syzygypl/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **The Nerdery**: [thenerdery/javascript-standards](https://github.com/thenerdery/javascript-standards)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **WeBox Studio**: [weboxstudio/javascript](https://github.com/weboxstudio/javascript)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ back to top](#table-of-contents)**

## Translation

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [yuche/javascript](https://github.com/yuche/javascript)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [ParkSB/javascript-style-guide](https://github.com/ParkSB/javascript-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnam**: [hngiang/javascript-style-guide](https://github.com/hngiang/javascript-style-guide)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)

## License

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team’s style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };

