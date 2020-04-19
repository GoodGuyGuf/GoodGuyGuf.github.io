---
layout: post
title:      "React.js: State & Component LifeCycle"
date:       2020-03-26 12:16:12 -0400
permalink:  react_js_state_and_component_lifecycle
---


Mentally, React is a little tough to understand. This blog post serves as a way to piece together what *State* and *LifeCycle Methods* actually do in my own words. 

***State***

State is data that is subject to change during a components life. State allows us to maintain and/or update information inside a component without needing its parent to send updated information. We can write state inside of a *constructor function*, since **it is the first function called when a component initializes.** Doing so would be setting the *initial state*. Setting state is *synchronous*.

We can write state inside a constructor function like so:

```
class MyState extends React.Component
constructor() {
    super()
    this.state = {
      myStateKey: "A Value"
    }
  }
	
	render() {
    return (
    <h1>Place something here</h1>
    )
  }
}
```

We have to call `super()` in the constructor since we are inheriting from the React Component by using the extends keyword. 


***`setState()`***

`setState()` is only used for one purpose: setting/updating state. `setState()` sets state *asynchronously*. We can write setState() like so:

```
this.setState({
      myStateKey:  "A New Value"
    })
```

***LifeCycle Methods***

React components go through something called a **lifecycle**. A components lifecycle has a three part process: *Creation*, *Updating* and *Deletion*. During the process we have access to lifecycle hooks(also called lifecycle methods).

They are called lifecycle methods, because they are called at different times in the component's lifecycle: before it's created, after it's created, and when it's about to be deleted.

***Mounting***

During the mounting phase, the constructor function gets called first. Then:

1. `static` `getDerivedStateFromProps` is called.

This method will give us access to any props and state, and can modify and return state before a component is rendered. `getDerivedStateFromProps` gets called every time a component renders, including the initial render and all following re-renders of content.

2. `render()` is called.

`render()` is invoked, returning JSX so that React can insert it into the DOM.

3. React updates the DOM
4. `componentDidMount()` is called.

`componentDidMount` is used for any long processes such as fetching content.

***Updating***

Whenever a component's state or props are changed, it gets re-rendered on the page.

During the updating phase:

1. `static` `getDerivedStateFromProps` is called.
2. `shouldComponentUpdate` is called.

`shouldComponentUpdate` is invoked just before the component is about to re-render. It will compare the old and new props and state and prevent unnecessary re-renders. If the props or state do not alter the component that the user will see, then there is no point of a re-render.

3. `render()` is called.
4. `getSnapShotBeforeUpdate` is called.

`getSnapShotBeforeUpdate` returns a '*snapshot*' that gets used in `componentDidUpdate`. It will let us capture things like scroll position.

5. React updates the DOM
6. `componentDidUpdate` is called.

From the React Documentation: `componentDidUpdate` is an opportunity to operate on the DOM when the component has been updated. It is also a good place to do network requests when comparing props to previous props.

***Unmounting***

During the unmounting phase:

1. `componentWillUnmount` is called.

`componentWillUnmount` clears anything set in `componentDidMount`.

In my final project, I was able to use `componentDidMount` for making a fetch requests at the top level of my application by using props connected by redux:

```
componentDidMount(){
    this.props.fetchPosts()
    this.props.fetchUsers()
  }
```

Both would be fetched and my children components would have all the necessary info while also avoiding potential re-renders that would happen if my children components did these fetches instead.
