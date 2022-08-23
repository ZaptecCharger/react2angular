# @ZaptecCharger/react2angular

A fork of [react2angular](https://github.com/coatue-oss/react2angular) adding support for child components, and browser-friendly builds.

This repo is configured to build an NPM package and deploy it to the GitHub NPM registry. To use the package as a dependency you will need to use a [Personal Access Token (PAT)](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry#authenticating-to-github-packages). Once you have a PAT, crete a `.nvmrc` file in your project with these contents:

```text
//npm.pkg.github.com/:_authToken=PAT_TOKEN
@zapteccharger:registry=https://npm.pkg.github.com
```

You can then add a dependency to `@zapteccharger/reactangular` via Yarn or NPM.

Below is the original README as found upstream:

<img alt="React to Angular: The easiest way to use React components in Angular 1" src="https://raw.githubusercontent.com/coatue-oss/react2angular/master/logo.png" width="400px" />

# react2angular [![Build Status](https://img.shields.io/circleci/project/coatue-oss/react2angular.svg?branch=master&style=flat-square)](https://circleci.com/gh/coatue-oss/react2angular) [![NPM](https://img.shields.io/npm/v/react2angular.svg?style=flat-square)](https://www.npmjs.com/package/react2angular) [![Apache2](https://img.shields.io/npm/l/react2angular.svg?style=flat-square)](https://opensource.org/licenses/Apache2)

> The easiest way to embed React components in Angular 1 apps! (opposite of [angular2react](https://github.com/coatue-oss/angular2react))

## Installation

```sh
# Using Yarn:
yarn add react2angular react react-dom prop-types

# Or, using NPM:
npm install react2angular react react-dom prop-types --save
```

## Usage

### 1. Create a React component

```js
import { Component } from 'react'

class MyComponent extends Component {
  render() {
    return <div>
      <p>FooBar: {this.props.fooBar}</p>
      <p>Baz: {this.props.baz}</p>
    </div>
  }
}
```

### 2. Expose it to Angular

```js
import { react2angular } from 'react2angular'

angular
  .module('myModule', [])
  .component('myComponent', react2angular(MyComponent, ['fooBar', 'baz']))
```

Note: If you defined [`propTypes`](https://facebook.github.io/react/docs/typechecking-with-proptypes.html) on your component, they will be used to compute component's bindings, and you can omit the 2nd argument:

```js
...
  .component('myComponent', react2angular(MyComponent))
```

If `propTypes` are defined and you passed in a 2nd argument, the argument will override `propTypes`.

### 3. Use it in your Angular 1 code

```html
<my-component
  foo-bar="3"
  baz="'baz'"
></my-component>
```

Note: All React props are converted to AngularJS one-way bindings. If you are passing functions into your React component, they need to be passed as a function ref, rather than as an invokable expression. Keeping an existing AngularJS-style expression will result in infinite loops as the function re-evaluates on each digest loop.

## Dependency Injection

It's easy to pass services/constants/etc. to your React component: just pass them in as the 3rd argument, and they will be available in your component's Props. For example:

```js
import { Component } from 'react'
import { react2angular } from 'react2angular'

class MyComponent extends Component {
  state = {
    data: ''
  }
  componentDidMount() {
    this.props.$http.get('/path').then(res =>
      this.setState({ data: res.data })
    )
  }
  render() {
    return <div>
      { this.props.FOO }
      { this.state.data }
    </div>
  }
}

angular
  .module('myModule', [])
  .constant('FOO', 'FOO!')
  .component('myComponent', react2angular(MyComponent, [], ['$http', 'FOO']))
```

Note: If you have an injection that matches the name of a prop, then the value will be resolved with the injection, not the prop.

## Tests

```sh
npm test
```

## License

Apache2
