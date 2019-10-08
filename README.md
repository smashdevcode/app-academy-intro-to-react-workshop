
# Intro to React

## Goal

* The goal of this workshop isn't to tell you everything that you need to know about React in order to be successful
* The goal is to give you a jumping off point to using and learning React

## Out of Scope

There's so much to talk about... something's got to go!

* Routing
* State Management
* Universal Rendering
* Mobile Development

## What We'll Cover

(Basic) Component Development

* Templating
* Properties
* Events

## Prerequisites

### Node

You'll need to have Node.js 6+ (and npm 5.2+) installed on your development machine in order to support the tooling that's typically used for React development.

You can use a package manager (like [Chocolately](https://chocolatey.org/) on Windows or [Homebrew](https://brew.sh/) on macOS) to install Node.js, or you can use the official installer available at [https://nodejs.org/en/](https://nodejs.org/en/).

### Create React App

In this workshop, we'll be using [Create React App](https://github.com/facebook/create-react-app) which doesn't require any installation.

### Browser DevTools

React has a browser extension available that extends the browser's built-in devtools:

* [react-devtools](https://github.com/facebook/react-devtools)

### Editor

I'll be using [Visual Studio Code](https://code.visualstudio.com/), but you're welcome to use any code editor or IDE that suits your needs.

## Getting Started

* create-react-app
* No installation necessary!
  * You CAN install it globally if you'd like, but the docs just recommend using npx

[User Guide](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md)

### Project Creation

```
npx create-react-app my-app
cd my-app
npm start
```

Then open the "src/App.js" file and change the code to:

```javascript
import React, { Component } from 'react';

export default function App() {
  return (
    <h1>Hello world!</h1>
  );
}
```

## Component Types

* Two types of components
  * Stateless functional components
  * Stateful class components

### Stateless Functional Components

* Stateless functional components are just ordinary functions
* A props object is passed into the function
  * This object gives you access to the properties that are set on the component
* Returns a React element
  * This example makes use of JSX
  * Syntax extension to JavaScript
  * JSX (after it's been transpiled by Babel) produces React "elements" (see the online Babel REPL at https://babeljs.io/repl/)

```javascript
function Album(props) {
  return (
    <div>
      <h2>{props.album.title}</h2>
    </div>
  );
}
```

### Stateful Class Components

The previous stateless functional component could be rewritten as a stateful class component:

```javascript
class Album extends Component {
  render() {
    return (
      <div>
        <h2>{this.props.album.title}</h2>
      </div>
    );
  }
}
```

But that example doesn't have any state... let's look at an example that has state:

```javascript
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      albums: []
    };
  }

  componentDidMount() {
    fetch('http://localhost:3000/albums')
      .then(response => response.json())
      .then(data => {
        this.setState({
          albums: data
        });
      });
  }

  render() {
    return (
      <div>
        {this.state.albums.map(album => (
          <Album album={album} key={album.id} />
        ))}
      </div>
    );
  }
}
```

## JSX

```javascript
function AlbumBox(props) {
  const album = props.album;

  return (
    <div className="column is-half">
      <div className="box">
        <div className="columns">
          <div className="column">
            <AlbumCover albumId={album.id} albumTitle={album.title} />
          </div>
          <div className="column">
            <h2 className="title">{album.title}</h2>
            <h3 className="subtitle">{album.artist}</h3>
            <div>
              Category: {album.category}
            </div>
          </div>      
        </div>      
      </div>
    </div>
  );
}
```

* JSX is not HTML!
* Element attributes mirror their JavaScript element properties
* How to create elements
  * HTML elements start with a lowercase letter
  * Components start with an uppercase letter
* How to set element attribute values

## Events

To handle an event, you wire up an event handler function or method to the element or component event you want to handle:

```javascript
function handleClick() {
  alert('Viewing details!');
}

function Album(props) {
  return (
    <div>
      <h2>{props.album.title}</h2>
      <button onClick={handleClick}>View Details</button>
    </div>
  );
}
```

Or if you were using stateful class components:

```javascript
class Album extends Component {
  handleViewDetails = () => {
    alert('Viewing details!');    
  }

  render() {
    return (
      <div>
        <h2>{this.props.album.title}</h2>
        <button onClick={this.handleViewDetails}>View Details</button>
      </div>
    );
  }
}
```

* Notice the use of property initializer syntax for the `handleViewDetails()` method
  * This approach is being used in order to workaround a problem that arises with `this` not being bound to the expected object instance
  * There are other ways to workaround "this" problem, including calling `bind()` on all class methods that will be used as event handlers

Or if you were passing an event handler down into a child component:

```javascript
function Album(props) {
  return (
    <div>
      <h2>{props.album.title}</h2>
      <button onClick={props.viewDetails}>View Details</button>
    </div>
  );
}

class App extends Component {

  ...

  handleViewDetails = () => {
    alert('View details!');
  }

  render() {
    return (
      <div>
        {this.state.albums.map(album => (
          <Album 
            album={album} 
            viewDetails={this.handleViewDetails} 
            key={album.id} />
        ))}
      </div>
    );
  }
}
```

## Props

* Props are readonly!

## State

* State must not be directly mutated
* Use `setState()` to mutate the state object

## Debugging

* Using Chrome's DevTools
  * Just set a breakpoint within any .js file in the "Sources" tab
* Chrome and Firefox browser extensions
  * [react-devtools](https://github.com/facebook/react-devtools)
* [Debugging in Visual Studio Code](https://code.visualstudio.com/docs/nodejs/reactjs-tutorial)

## Production Build

```
npm run build
```

React generates files into a `build` folder

## Completed Examples

The "Basic" version is the version that we built in this exercise. The "Complete" version implements a basic search feature and includes styles and images.

* [Basic](/demos/basic)
* [Complete](/demos/complete)

## Additional Practice

* Build a version of the AirBench bench list with filtering

## Additional Reading

* See the App Academy app for additional content about React
* Also see the [React docs](https://reactjs.org/docs/)
