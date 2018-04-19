# Airbnb React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions (i.e async/await or static class fields) may still be included or prohibited on a case-by-case basis. Currently, anything prior to stage 3 is not included nor recommended in this guide.

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Mixins](#mixins)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## Basic Rules

  - 每个文件只包含一个反应组件。
  -  然而，每个文件都允许多个 [无状态或纯粹, 组件].(https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  -  总是使用JSX语法。
  - 不要使用 `React.createElement` 除非你是从一个不是JSX的文件中初始化应用程序。

## Class vs `React.createClass` vs stateless

  - 如果你有内部状态和/或refs，更喜欢 `class extends React.Component` 在 `React.createClass`组件。 eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // 不应该使用
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // 推荐使用
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    如果你没有state或refs，你更喜欢正常的函数（不是箭头函数）而不是类：

    ```jsx
    // 不应该使用
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // 不应该使用 (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // 推荐使用
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Mixins

  - [不要使用mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).
  
  为什么?mixin引入了隐式依赖关系，导致名称冲突，并导致滚雪球式的复杂性。mixin的大多数用例都可以通过组件、高阶组件或实用模块更好地完成。

## Naming

  - **扩展**:使用 `.jsx` 扩展作为反应组件。
  - **文件名**: 用PascalCase命名文件名。 例如, `ReservationCard.jsx`.
  - **引用命名**: 使用PascalCase为其实例进行反应组件和camelCase。 eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // 不应该使用
    import reservationCard from './ReservationCard';

    // 推荐使用
    import ReservationCard from './ReservationCard';

    // 不应该使用
    const ReservationItem = <ReservationCard />;

    // 推荐使用
    const reservationItem = <ReservationCard />;
    ```

  - **组件命名**: 使用文件名作为组件名称。例如, `ReservationCard.jsx` 应该有一个 `ReservationCard`的参考名。但是，对于目录的根组件，使用 `index.jsx` 作为文件名，并使用目录名作为组件名称：

    ```jsx
    // 不应该使用
    import Footer from './Footer/Footer';

    // 不应该使用
    import Footer from './Footer/index';

    // 推荐使用
    import Footer from './Footer';
    ```
  - **高阶组件命名**: 使用高阶组件的名称和已传入组件的名称作为生成组件上的 `displayName` 例如，带有`withFoo()`, 的高阶组件，当传递一个组件条时，应该生成带有withFoo `Bar` 的`displayName`的组件。

    为什么?组件的`displayName`可以被开发人员工具或错误消息使用，并且拥有一个清晰地表达这种关系的值可以帮助人们理解正在发生的事情。

    ```jsx
    // 不应该使用
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // 推荐使用
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Props命名**: 为了不同的目的避免使用DOM组件Props names 。

    为什么?人们期望像`style`和`className`这样的道具代表一种特定的东西。为应用程序的子集改变这个API，使代码的可读性更差，更难以维护，并且可能导致bug。

    ```jsx
    // 不应该使用
    <MyComponent style="fancy" />

    // 不应该使用
    <MyComponent className="fancy" />

    // 推荐使用
    <MyComponent variant="fancy" />
    ```

## Declaration

  - 不要使用`displayName`来命名组件。相反，通过引用来命名组件。

    ```jsx
    // 不应该使用
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // 推荐使用
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - 按照这些对齐方式进行JSX语法。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```jsx
    // 不应该使用
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // 推荐使用
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## Quotes

  - 总是使用双引号 (`"`) 为JSX属性，但是对于所有其他的JS都使用单引号 (`'`)  eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    为什么?常规的HTML属性通常使用双引号而不是单引号，所以JSX属性反映了这个约定。

    ```jsx
    // 不应该使用
    <Foo bar='bar' />

    // 推荐使用
    <Foo bar="bar" />

    // 不应该使用
    <Foo style={{ left: "20px" }} />

    // 推荐使用
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  -  总是在你的自闭标签中包含一个空格。 eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // 不应该使用
    <Foo/>

    // very bad
    <Foo                 />

    // 不应该使用
    <Foo
     />

    // 推荐使用
    <Foo />
    ```

  -  不要用空格键填充JSX花括号。 eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)
 
    ```jsx
    // 不应该使用
    <Foo bar={ baz } />

    // 推荐使用
    <Foo bar={baz} />
    ```

## Props

  - 总是用camelCase(驼峰）来对prop命名。

    ```jsx
    // 不应该使用
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // 推荐使用
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - 当它明确为`true`时，省略该prop 的值。eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // 不应该使用
    <Foo
      hidden={true}
    />

    // 推荐使用
    <Foo
      hidden
    />

    // 推荐使用
    <Foo hidden />
    ```

  - 总是在 `<img>`标签上加上一个`alt`属性。 如果图像是表示形式的， `alt` 可以是空字符串， 或者 `<img>` 必须有 `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```jsx
    // 不应该使用
    <img src="hello.jpg" />

    // 推荐使用
    <img src="hello.jpg" alt="Me waving hello" />

    // 推荐使用
    <img src="hello.jpg" alt="" />

    // 推荐使用
    <img src="hello.jpg" role="presentation" />
    ```

  -   不要在`<img>` `alt`中使用“图片”、“照片”或“图片”这样的词。 eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    为什么?screenreader已经宣布`img`元素为图像，因此不需要在alt文本中包含这些信息。

    ```jsx
    // 不应该使用
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // 推荐使用
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - 只使用有效的、非抽象的 [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions) 角色。 eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // 不应该使用 - not an ARIA role
    <div role="datepicker" />

    // 不应该使用 - abstract ARIA role
    <div role="range" />

    // 推荐使用
    <div role="button" />
    ```

  - 不要在元素上使用`accessKey`。 eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)
  

  为什么?使用屏幕阅读器和键盘的人使用键盘快捷键和键盘命令之间的不一致性使可访问性变得复杂。

  ```jsx
  // 不应该使用
  <div accessKey="h" />

  // 推荐使用
  <div />
  ```

  -  避免使用数组索引作为`key`支柱，更喜欢唯一的ID。 ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))
 
  ```jsx
  // 不应该使用
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // 推荐使用
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - 总是为所有非必需的道具定义显式的默认props。

  为什么?proptype是一种文档形式，提供默认的支持意味着您的代码的读者不必承担太多的假设。此外，它还意味着您的代码可以省略某些类型检查。

  ```jsx
  // 不应该使用
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // 推荐使用
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

  - 有节制的使用props传播。
  > Why? Otherwise you're more likely to pass unnecessary props down to components. And for React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).
  
  为什么?否则，你就更有可能将不必要的props传递给组件。对于v15.6.1和更高版本的响应，您可以将无效的HTML属性传递给DOM。

  Exceptions:异常

  - HOCs that proxy down props and hoist propTypes

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
  ```

  - 用已知的、显式的props传播物体。这在测试与Mocha在每个构造之前的反应组件时特别有用。

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  ```

  使用说明：
    在可能的情况下过滤掉不必要的props。另外，使用 [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) 类型来帮助防止bug。

  ```jsx
  // 推荐使用
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...relevantProps} />
  }

  // 不应该使用
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...this.props} />
  }
  ```

## Refs

  -  总是使用ref回调。 eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)
 
    ```jsx
    // 不应该使用
    <Foo
      ref="myRef"
    />

    // 推荐使用
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## Parentheses

  -   当它们跨越多个行时，将JSX标记封装在括号中。 eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```jsx
    // 不应该使用
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // 推荐使用
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // 推荐使用, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - 总是自闭标签，没有孩子。eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // 不应该使用
    <Foo variant="stuff"></Foo>

    // 推荐使用
    <Foo variant="stuff" />
    ```

  -   如果你的组件有多行属性，那么在新行上关闭它的标签。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)


    ```jsx
    // 不应该使用
    <Foo
      bar="bar"
      baz="baz" />

    // 推荐使用
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - 使用箭头函数来关闭局部变量。

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  -   在构造函数中为render方法绑定事件处理程序。 eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)


    为什么?渲染路径中的绑定调用在每一个渲染上创建一个全新的函数。

    ```jsx
    // 不应该使用
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // 推荐使用
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

  - 不要在反应组件的内部方法中使用下划线前缀。

   为什么?下划线前缀有时被用作其他语言的约定，以表示隐私。但是，与这些语言不同的是，在JavaScript中没有原生的隐私支持，一切都是公开的。不管您的意图是什么，在属性中添加下划线前缀并不会使它们成为私有的，并且任何属性（未被修改的前缀）都应该被视为公开的。见问题[#1024](https://github.com/airbnb/javascript/issues/1024)和 [#490](https://github.com/airbnb/javascript/issues/490)，进行更深入的讨论。

    ```jsx
    // 不应该使用
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // 推荐使用
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - 一定要在`render`方法中返回一个值。 eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)
  

    ```jsx
    // 不应该使用
    render() {
      (<div />);
    }

    // 推荐使用
    render() {
      return (<div />);
    }
    ```

## Ordering

  - Ordering for `class extends React.Component`:

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - 如何定义 `propTypes`, `defaultProps`, `contextTypes`, 等...

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - 不要使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > 为什么? [`isMounted` 是一种反模式][anti-pattern],在使用ES6类时不可用，并且正在被正式弃用。

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Translation

  This JSX/React style guide is also available in other languages:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript/tree/master/react)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Español**: [agrcrobles/javascript](https://github.com/agrcrobles/javascript/tree/master/react)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb/tree/master/react)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide/tree/master/react)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [alioguzhan/react-style-guide](https://github.com/alioguzhan/react-style-guide)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript/tree/master/react)

**[⬆ back to top](#table-of-contents)**
