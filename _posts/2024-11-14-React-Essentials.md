---
title: "React Essentials"
categories:
image: 
  path: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*2qvsKGdGxlFl_Q8Tfp9njg.png
  thumbnail: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*2qvsKGdGxlFl_Q8Tfp9njg.png
tags:
  - design pattern
  - UI
---

React Essentials
================


BuildingBlocks
==============

**Element**: An immutable plain object, also the smallest building blocks of React.

**JSX**:it allows writing HTML in JavaScript and converts the HTML tags into a React **Element**, which can later be rendered to the DOM.

**Pure Function**: a javasciprt function that never modify its own props.

**Component:** a javascript function that returns React Elements for rendering. (**All React components must act like pure functions with respect to their props.)**

**props**: input parameters passed into a React Component.

**State**: private properties fully controlled by a React Component.

LifeCycle Methods
=================

**componentDidMount()**: method runs after the component output has rendered to the DOM.

**componentWillUnmount()**: method runs when component is removed from the DOM.

Rendering
=========

**SetState()**: A method that may execute asynchonously and trigger React render method to know what should be on the screen. It compares new DOM tree with last DOM tree and make updates only to the changed part.

State Updates May Be Asynchronous
=================================

React may batch multiple `setState()` calls into a single update for performance.

Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.

For example, this code may fail to update the counter:

```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// With object form of setState (potential issue):
this.setState({ counter: this.state.counter + 1 });
this.setState({ counter: this.state.counter + 1 });
```

The second this.state.counter might still be the old value.

Rendering Flow:

1.  a single setState() -> re-rendering request -> update DOM(one time) ->update this.state
2.  multiple setState() calls in a very short time interval -> batch the changes -> update DOM (one time) -> update this.state

**Because this.state is updated after DOM updates, the state in multiple setState() calls refer to the same old value.**

To fix it, use a second form of `setState()` that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

```
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

The way to understand this is the key to understand React. Think about what will be next to show instead of what changes have been made.

**Complete Example**

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
  tick() {
    this.setState({
      date: new Date()
    });
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Clock />);
```

**Summary**

These are fundemental blocks that build up the structure and render fucntions in React. The materials are collected from React Offical Documentaiton. Please go to [https://legacy.reactjs.org/docs/getting-started.html](https://legacy.reactjs.org/docs/getting-started.html) for more information.

Thank you for reading this! Feel free to leave any Comment!