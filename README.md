# REACT-REFERENCE
This repository is a personal reference for React concepts and snippets of code that are used daily.

:page_facing_up: - SUMMARY
<ul>
  <li>MAIN DOCUMENTATION CONCEPTS (CLASS COMPONENTS)</li>
</ul>

#### MAIN DOCUMENTATION CONCEPTS (CLASS COMPONENTS)

React elements are immutable. Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

The simplest way to define a component is to write a JavaScript function:

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

This function is a valid React component because it accepts a single “props” (which stands for properties) object argument with data and returns a React element. We call such components “function components” because they are literally JavaScript functions.

You can also use an ES6 class to define a component:

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The above two components are equivalent from React’s point of view.

We recommend naming props from the component’s own point of view rather than the context in which it is being used.

<b>PROPS ARE READ ONLY, doesnt matter of the component is a function or a class</b>

PURE FUNCTION

```javascript
function sum(a, b) {
  return a + b;
}
```

IMPURE FUNCTION

```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```

<b>All React components must act like pure functions with respect to their props.

Converting a function to a class

You can convert a function component like Clock to a class in five steps:</b>

<ol>
  <li>Create an ES6 class, with the same name, that extends React.Component</li>
  <li>Add a single empty method to it called render()</li>
  <li>Move the body of the function into the render() method</li>
  <li>Replace props with this.props in the render() body</li>
  <li>Delete the remaining empty function declaration</li>
</ol>

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

<b>Adding  Local State to a Class</b>

We will move the date from props to state in three steps

1. Replace `this.props.date` with `this.state.date in` the render() method:

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>

       <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
     </div>
    );
  }
}
```

2. Add a class constructor that assigns the initial this.state:

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props); //ATTENTION TO THIS
    this.state = {date: new Date()};
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
```

Class components should always call the base constructor with props.

3. Remove the date prop from the `<Clock/>` element:

```javascript
root.render(<Clock />);
```

<b>Adding lifecycle methods to a class</b>

In applications with many components, it’s very important to free up resources taken by the components when they are destroyed.

We want to set up a timer whenever the Clock is rendered to the DOM for the first time. This is called “mounting” in React.

We also want to clear that timer whenever the DOM produced by the Clock is removed. This is called “unmounting” in React.

We can declare special methods on the component class to run some code when a component mounts and unmounts:

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }


 componentDidMount() {
 }


 componentWillUnmount() {
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
```

These methods are called “lifecycle methods”.

The componentDidMount() method runs after the component output has been rendered to the DOM. This is a good place to set up a timer:

```javascript
 componentDidMount() {

   this.timerID = setInterval(
     () => this.tick(),
     1000
   );
 }

 componentWillUnmount() {

   clearInterval(this.timerID);
 }
```

Finally, we will implement a method called tick() that the Clock component will run every second.

It will use this.setState() to schedule updates to the component local state:

```javascript
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

The only place where you can assign this.state is the constructor.

<b>Set State Updates May Be Asynchronous</b>

React may batch multiple setState() calls into a single update for performance.

Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state

For example, this code may fail to update the counter:

```javascript
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});

To fix it, use a second form of setState() that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

<b>State Updates are Merged</b>

When you call setState(), React merges the object you provide into the current state.

<b>The Data Flows Down</b>

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

<b>Handling Events</b>

React events are named using camelCase, rather than lowercase. For example:

HTML

```javascript
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

REACT 

<button onClick={activateLasers}>
  Activate Lasers
</button>

When you define a component using an ES6 class, <b>a common pattern is for an event handler to be a method on the class.</b> For example, this Toggle component renders a button that lets the user toggle between “ON” and “OFF” states:

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

You have to be careful about the meaning of this in JSX callbacks. In JavaScript, class methods are not bound by default. If you forget to bind this.handleClick and pass it to onClick, this will be undefined when the function is actually called.

This is not React-specific behavior; it is a part of how functions work in JavaScript. Generally, <b>if you refer to a method without () after it</b>, such as onClick={this.handleClick}, you should bind that method.

```javascript
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  handleClick = () => {
    console.log('this is:', this);
  };
  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

If you aren’t using class fields syntax, you can use an arrow function in the callback:

```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

The problem with this syntax is that a different callback is created each time the LoggingButton renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. <b>We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.</b>

<b>Passing Arguments to Event Handlers</b>

Inside a loop, it is common to want to pass an extra parameter to an event handler. For example, if id is the row ID, either of the following would work:

```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

The above two lines are equivalent, and use arrow functions and Function.prototype.bind respectively.

In both cases, the e argument representing the React event will be passed as a second argument after the ID. With an arrow function, we have to pass it explicitly, but with bind any further arguments are automatically forwarded.

<b>Conditional Rendering</b>

```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    //binding on constructor
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<LoginControl />);
```

Using ternary to conditional render

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

Just like in JavaScript, it is up to you to choose an appropriate style based on what you and your team consider more readable. 

<b>VERY GOOD TIP:
Also remember that whenever conditions become too complex, it might be a good time to extract a component.</b>

<b>Preventing Component from Rendering</b>

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return null instead of its render output.

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

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(state => ({
      showWarning: !state.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<Page />);
```

Returning null from a component’s render method does not affect the firing of the component’s lifecycle methods. For instance componentDidUpdate will still be called.

<b>Keys</b>

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

We don’t recommend using indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state.

<b>Keys Must Only Be Unique Among Siblings</b>

Keys used within arrays should be unique among their siblings. However, they don’t need to be globally unique. We can use the same keys when we produce two different arrays:

Keys serve as a hint to React but they don’t get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name:

```javascript
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

With the example above, the Post component can read props.id, but not props.key.

<b>Forms</b>

HTML form elements work a bit differently from other DOM elements in React, because form elements naturally keep some internal state. For example, this form in plain HTML accepts a single name:

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

This form has the default HTML form behavior of browsing to a new page when the user submits the form. If you want this behavior in React, it just works. But in most cases, it’s convenient to have a JavaScript function that handles the submission of the form and has access to the data that the user entered into the form. The standard way to achieve this is with a technique called “controlled components”.

<b>Controled Components</b>
