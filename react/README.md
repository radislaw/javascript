# Airbnb React/JSX Style Guide

*Самый разумный подход в React и JSX*

## Table of Contents

  1. [Основные правила](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Именование](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Кавычки](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Очередность](#ordering)
  1. [`isMounted`](#ismounted)

## Основные правила

  - Включайте только один компонент React на файл.
    - Однако, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Всегда используйте синтаксис JSX.
  - Не используйте `React.createElement` unless you're initializing the app from a file that is not JSX.

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass` unless you have a very хорошо reason to Используйте mixins. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // плохо
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // хорошо
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    And if you don't have state or refs, prefer normal functions (not arrow functions) over classes:

    ```jsx
    // плохо
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // плохо (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // хорошо
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Naming

  - **Расширение**: Используйте расширение `.jsx` для компонентов React.
  - **Именование файлов**: Используте PascalCase при именовании файлов. Например, `ReservationCard.jsx`.
  - **Reference Naming**: Используйте PascalCase для компонентов React и camelCase для их экземпляров. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // плохо
    import reservationCard from './ReservationCard';

    // хорошо
    import ReservationCard from './ReservationCard';

    // плохо
    const ReservationItem = <ReservationCard />;

    // хорошо
    const reservationItem = <ReservationCard />;
    ```

  - **Именование компонентов**: Используйте имя файла в качестве имени компонента. Например, `ReservationCard.jsx` should have a reference name of `ReservationCard`. Однако, for root components of a directory, Используйте `index.jsx` as the filename and Используйте the directory name as the component name:

    ```jsx
    // плохо
    import Footer from './Footer/Footer';

    // плохо
    import Footer from './Footer/index';

    // хорошо
    import Footer from './Footer';
    ```

## Declaration

  - Do not Используйте `displayName` for naming components. Instead, name the component by reference.

    ```jsx
    // плохо
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // хорошо
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // плохо
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // хорошо
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

## Кавычки

  - Всегда используйте двойные кавычки (`"`) для аттрибутов JSX, и одинарные кавычки для всего прочего JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > Почему? JSX attributes [can't contain escaped quotes](http://eslint.org/docs/rules/jsx-quotes), so double quotes make conjunctions like `"don't"` easier to type.
  > Regular HTML attributes also typically Используйте double quotes instead of single, so JSX attributes mirror this convention.

    ```jsx
    // плохо
    <Foo bar='bar' />

    // хорошо
    <Foo bar="bar" />

    // плохо
    <Foo style={{ left: "20px" }} />

    // хорошо
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag.

    ```jsx
    // плохо
    <Foo/>

    // very плохо
    <Foo                 />

    // плохо
    <Foo
     />

    // хорошо
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // плохо
    <Foo bar={ baz } />

    // хорошо
    <Foo bar={baz} />
    ```

## Props

  - Always Используйте camelCase for prop names.

    ```jsx
    // плохо
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // хорошо
    <Foo
      UserName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // плохо
    <Foo
      hidden={true}
    />

    // хорошо
    <Foo
      hidden
    />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // плохо
    <img src="hello.jpg" />

    // хорошо
    <img src="hello.jpg" alt="Me waving hello" />

    // хорошо
    <img src="hello.jpg" alt="" />

    // хорошо
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not Используйте words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > Почему? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // плохо
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // хорошо
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Используйте only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // плохо - not an ARIA role
    <div role="datepicker" />

    // плохо - abstract ARIA role
    <div role="range" />

    // хорошо
    <div role="button" />
    ```

  - Do not Используйте `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Почему? Inconsistencies between keyboard shortcuts and keyboard commands Используйтеd by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // плохо
  <div accessKey="h" />

  // хорошо
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a unique ID. ([Почему?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // плохо
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // хорошо
  {todos.map((todo) =>
    <Todo
      {...todo}
      key={todo.id}
    />
  )}
  ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // плохо
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // хорошо
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // хорошо, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // плохо
    <Foo className="stuff"></Foo>

    // хорошо
    <Foo className="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // плохо
    <Foo
      bar="bar"
      baz="baz" />

    // хорошо
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - Используйте arrow functions to close over local variables.

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

  - Bind event handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Почему? A bind call in the render path creates a brand new function on every single render.

    ```jsx
    // плохо
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // хорошо
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - Do not Используйте underscore prefix for internal methods of a React component.

    ```jsx
    // плохо
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // хорошо
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Be sure to return a value in your `render` methods. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // плохо
    render() {
      (<div />);
    }

    // хорошо
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
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```jsx
    import React, { PropTypes } from 'react';

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
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
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
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Do not Используйте `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Почему? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Translation

  This JSX/React style guide также доступен на других языках:

  - ![cn](https://raw.githubUsercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)

**[⬆ вверх](#table-of-contents)**
