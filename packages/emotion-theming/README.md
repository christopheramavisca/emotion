# emotion-theming

> A CSS-in-JS theming solution for React

_`emotion-theming` is a theming library inspired by [styled-components](https://github.com/styled-components/styled-components)_

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [API](#api)
  - [ThemeProvider](#themeprovider)
  - [withTheme](#withthemecomponent)
- [Credits](#credits)
- [License](#license)

## Install

```bash
# add --save if using npm@4 or lower
npm i emotion-theming

# or
yarn add emotion-theming
```

## Usage

### TODO: Add example with the css prop

Theming is accomplished by placing the `ThemeProvider` component, at the top of the React component tree and wrapping descendants with the `withTheme` higher-order component (HOC). This HOC seamlessly acquires the current theme and injects it as a "prop" into your own component.

The theme prop is automatically injected into components created with `styled`.

Here is a complete example for a typical React + Emotion app (information about each piece of the theming API is listed afterward):

```jsx
/** child.js */
import React from 'react'
import styled from 'react-emotion'

const Container = styled.div`
  background: whitesmoke;
  height: 100vh;
`

const Headline = styled.h1`
  color: ${props => props.theme.color};
  font: 20px/1.5 sans-serif;
`

export default class Page extends React.Component {
  render() {
    return (
      <Container>
        <Headline>I'm red!</Headline>
      </Container>
    )
  }
}

/** parent.js */
import React from 'react'
import ReactDOM from 'react-dom'
import { ThemeProvider } from 'emotion-theming'

import Page from './child.js'

const theme = {
  color: 'red'
}

class App extends React.Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <Page />
      </ThemeProvider>
    )
  }
}

// this assumes the HTML page template has a <main> element already inside <body>
ReactDOM.render(<App />, document.querySelector('main'))
```

`<ThemeProvider>` acts as a conductor in the component hierarchy and themed components receive the `theme` for whatever purposes are necessary, be it styling or perhaps toggling a piece of functionality.

## API

### ThemeProvider: React.ComponentType

A React component that passes the theme object down the component tree via [context](https://reactjs.org/docs/context.html). Additional `<ThemeProvider>` wrappers may be added deeper in the hierarchy to override the original theme. The theme object will be merged into its ancestor as if by [`Object.assign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign). If a function is passed instead of an object it will be called with the ancestor theme and the result will be the new theme.

_Accepts:_

- **`children`: ReactComponent** - A single React component.
- **`theme`: Object|Function** - An object or function that provides an object.

```jsx
import React from 'react'
import styled from 'react-emotion'
import { ThemeProvider, withTheme } from 'emotion-theming'

// object-style theme

const theme = {
  backgroundColor: 'green',
  color: 'red'
}

// function-style theme; note that if multiple <ThemeProvider> are used,
// the parent theme will be passed as a function argument

const adjustedTheme = ancestorTheme => ({ ...ancestorTheme, color: 'blue' })

class Container extends React.Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <ThemeProvider theme={adjustedTheme}>
          <Text>Boom shaka laka!</Text>
        </ThemeProvider>
      </ThemeProvider>
    )
  }
}

const Text = styled.div`
  background-color: ${props => props.theme.backgroundColor}; // will be green
  color: ${props => props.theme.color}; // will be blue
`
```

### withTheme(component: React.ComponentType): React.ComponentType

A higher-order component that provides the current theme as a prop to the wrapped child and listens for changes. If the theme is updated, the child component will be re-rendered accordingly.

```jsx
import PropTypes from 'prop-types'
import React from 'react'
import { withTheme } from 'emotion-theming'

class TellMeTheColor extends React.Component {
  render() {
    return <div>The color is {this.props.theme.color}.</div>
  }
}

TellMeTheColor.propTypes = {
  theme: PropTypes.shape({
    color: PropTypes.string
  })
}

const TellMeTheColorWithTheme = withTheme(TellMeTheColor)
```

## Credits

Thanks goes to the [styled-components team](https://github.com/styled-components/styled-components) and [their contributors](https://github.com/styled-components/styled-components/graphs/contributors) who originally wrote this.

## License

MIT 2017-present
