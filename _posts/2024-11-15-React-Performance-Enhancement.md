---
title: "React Performance enhancement"
categories:
image: 
  path: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*bdvQaZIn0hCCG4O_XnRxsA.png
  thumbnail: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*bdvQaZIn0hCCG4O_XnRxsA.png
tags:
  - design pattern
  - UI
---
React Performance enhancement
=============================

In the last post, I talked about React essentials.
 [React Essentials](https://daniel13520cs.github.io/React-Essentials/)

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*bdvQaZIn0hCCG4O_XnRxsA.png)

In this article, I will explain how data flows in a React application, triggers re-rendering, and explore ways to optimize React re-rendering performance.

Parent and Children Component Interactions
==========================================

When I first designed a React application, I encountered difficulties in implementing a way to pass data from a child component to a parent component. However, I soon discovered that using a callback function was an intuitive solution, and it’s a common approach found in many sample codes.

```
import React, { useState } from 'react';
import Child from './Child';
export default function App() {
  const [data, setData] = useState('');
  // This function will be passed down to the child component
  const handleDataFromChild = (childData) => {
    setData(childData); // Update parent state with the data from the child
  };
  return (
    <div>
      <h1>Parent Component</h1>
      <p>Data received from child: {data}</p>
      {/* Pass the function as a prop to the child */}
      <Child onDataChange={handleDataFromChild} />
    </div>
  );
}
```

The `setState` method(setData in this example) can be placed inside the callback function to trigger re-rendering of both the parent and child components, propagating the data from top to bottom, which is how data flow should occur in React.

Re-Rendering
============

React has a built-in virtual DOM algorithm that compares and identifies the changes, then updates only the affected parts of the actual DOM tree. However, this does not reduce the number of re-render calls.

By default, all child components re-render automatically when the state of a parent component changes. The optimization to apply here is to use memo API provided by REACT.

```
import { memo, useState } from 'react';
export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}
const Greeting = memo(function Greeting({ name }) {
  console.log("Greeting was rendered at", new Date().toLocaleTimeString());
  return <h3>Hello{name && ', '}{name}!</h3>;
});
```

Let’s first look at the example provided in the official React documentation. When the address is updated by the user, the `Greeting` child component does not re-render because it is wrapped with the `memo` API. Since the props of `Greeting` do not include the `address`, changes to the `address` do not trigger a re-render of the `Greeting` component.

```
import { memo, useState } from 'react';
export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      {/* Pass name and address as separate props */}
      <Greeting name={name} address={address} />
    </>
  );
}
const Greeting = memo(function Greeting({ name, address }) {
  console.log("Greeting was rendered at",name, address, new Date().toLocaleTimeString());
  return <h3>Hello{name && ', '}{name}! Hello{address && ', '}{address}!</h3>;
});
```

Now, let’s look at another example where the `Greeting` component contains both `name` and `address` in its props. Does a change to the `address` trigger a re-render of the `Greeting` component? The answer is **YES**!

The reason is that any changes made to the props of a component will cause the component to re-render. This makes sense because the component needs to update according to the new context or data passed through its props.

Thanks you for reading this post! Please leave any comments if you like!