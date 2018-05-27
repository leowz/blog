---
title: ReactBasics
date: 2018-05-20 15:36:06
tags: react
categories: web
---

# Components and Props
Components are linke JS functions, they accept inputs called props and return React elements to render on screen.

## Functional and Classy Components
A JS function can define a React component.

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
Its a valid React component because it accepts a single props object argument and returns a React element. Its called a functional components.

We can also define a class that extends `React.Component`. These two components are equivalent from React's point of view.


## Props
When React sees an JSX element with attributes, it passes this attributes to this component as a single object. We call this object props.

example:
```javascript 
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

In this example:
1. We call ReactDom.render() function with `<Welcome name=’Sara’ />` element.
2. React calls the Welcome component with `{name: ’Sara’}` as the props, since class or React component is in fact a function.
3. Welcome component returns a `<h1 >Hello, Sara</ h1>` element as the result.
4. React DOM updates the DOM to display the added h1 tag.

Note: Always start component names with a capital letter. Since React treated components starting with lowercase letters as DOM(HTML) tags.


<!--more-->
## Props are Read-Only
React has a single strict rule: All React components must act like pure functions and do not change their inputs aka props.

# State and Lifecycle
`state` is an instance property for the React.Component class. The state is user-defined, plain JS object. Local state is a feature available only the component is defined as a class.

State contains data specific to this component that may change over time. If the value is not used for rendering or data flow, you don’t put it in the state. It can be defined as other fields on the component.

Never change the value of state directly, use the instance method `setState()` instead. Treat `this.state` as if it were immutable.

## setState()
`setState` enqueues changes to the component state and tells React that this component and its children need to be re-rendered with the update state.

Think of setState as a request rather than an immediate update command, because it does not always update the component immediately. Since that, reading `this.state` right after calling setState might cause bugs. To do that, pass a callback and read `this.state` in the callback to guarantee the state in the corrected value.

setState will always lead to a re-render. So call it if only necessary to avoid too many re-renders.

1. `setState(updater[, callback])`

The first argument is an updater function with the signature `(prevState, props) => {setChanges}`.
setChanges must return a changed JS object for new state.
The callback will be fired after the successful update of state.

2. `setState(stateChange[, callback])`

Alternatively,  we can pass an object representing changed state instead of a function.
This performs a shallow merge of stateChange into the new state, meaning it only changes or insert the key value paris in the stateChange object and leave other key values in `this.state` untouched.

## Component Life cycle 

<img  src="https://cdn-images-1.medium.com/max/2000/1*sn-ftowp0_VVRbeUAFECMA.png">
<br />

Each component has several lifecycle methods that you can override to run code at particular times.

### Mounting
These methods are called subsequently when an instance of a component is being created and inserted into the DOM:

* constructor()
* static getDerivedStateFromProps()
* componentWillMount()
* render()
* componentDidMount()

**render():**
The render method is required in the component class.

When called, it examines `this.props` and `this.state` and return one of the following types:
* React elements:    JSX elements used to render tags in the DOM.
* Strings and/or numbers:   rendered as text nodes in the DOM.
* portals:  created with `ReactDOM.createPortal`.
* null/Booleans:    render nothing. 

Note: render() will not be invoked if `shouldCOmponentUpdate()` returns false.

**constructor:**
This method is called before it is mounted. You should call super(props) before any other statement.
The constructor is the right place to initialise state. It is the only place to set this.state value. Do not use `setState()` in the constructor. 

**static getDerivedStateFromProps(nextProps, prevState):**
invoked after a component is instantiated or when it receives new props.

**componentWillMount() & componentDidMount():**
`WillMount` just occurs before mounting and before render() is called.

`DidMount` is invoked immediately after a component is mounted. If you need to load data from remote endpoint, this is a good place to instantiate the network request.

### Updating 
An update can be caused by changes to props or state. The following methods are called when a component needs a re-render.

* componentWillReceivedProps()
* static getDerivedStateFromProps()
* shouldComponentUpdate()
* componentWillUpdated()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()

**shouldComponentUpdate(nextProps, nextState):**
shouldComponentUpdate is invoked before rendering when new props or state are being received. This method is not called for the initial render or when forceUpdate is used.

Default return true, if returns false, component will update ,render and component did update methods will not invoke.

**componentWillUpdate():**
WillUpdate is invoked just before rendering when new props or state are being received. 

Do not call setState here. If you need to update state in response to props changes, use getDerivedStateFromProps instead.

**getSnapshotBeforeUpdate():**
Invoked right before the most recently rendered output. It enables your component to capture current values before they are potentially changed. Value return by this method will be passed to componentDidUpdate.

**componentDidUpdate():**
Invoked immediately after updating. this method is not called for the initial render.

### Unmounting
When a component is being removed from the DOM.
* componentWillUnmount()

### Error Catched
When there is an error occurs
* componentDidCatch()

# Handling Events
Handing events with React element is similar to that of DOM elements with a few differences.

* React events are name using camelCase, rather than lowercase.
* JSX pass a function as the event handler, not a string.
* You must call `event.preventDefault()` to prevent default behavior, instead of return false.

React events are defined according to the W3C spec. The event handler have a parameter called event.

## this bind
In JS, class methods ar not bound by default. So if you use `this` in a event handler without binding `this` to this handler, this will be undefined. There are some solution to this in React.

1. use bind function method to bind this to the event handler.

example:
```javascript
constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }
```

2. public class fields syntax (recommended)

example:
```javascript
 // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }
```

3. arrow function in the callback

example:
```javascript
  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
```

## Passing Arguments to Event Handlers
It is common to pass extra parameters to an event handler.

```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

The above two lines are the same. First use arrow functions, and second use bind method.

In both case, event argument representing the React event will be passed as a second argument after id. With arrow functions, we have to do it explicitly, but with bind any further arguments are automatically forwarded.

# Conditional Rendering

## Inline If with && Operator
We can embed any expressions in JSX by wrapping them in curly braces. This includes JS logical operator &&. And it server as a if statement.

`condition && expression`

example:
```javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
```
Only if the condition is true the expression will be evaluated.

## Inlin If-Else
Another operator yo use in JS.
`condition ? expression true : expression false.`

example:
```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

## Preventin Component from Rendering
Inside a component, we can prevent a component from being rendered by return `null` in some condition.

A component the returns `null` will not be rendered by the parent component.

example:
```javascript
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }
  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```
Returning null form a component`s render method does not affect the firing of the component`s lifecycle methods. For instance, `componentWillUpdate` and `componentDidUpdate` will still be called.

# Forms
In React, we can add more control on the form, a part from its original functionalities.

## Controlled Components 
An input form element whose value is controlled by React is called a controlled component.

Elements like input text area and select have their own state based on user input, we can pass a event handler to control the state.

## Handling Multiple inputs
When using multiple inputs with the some handler function. We can choose the different component base on the value of `event.target.name` in the event handler. This first require setting the name attribute of those inputs. In this way, in the event handler function, we can differentiate different value of inputs by their names.
