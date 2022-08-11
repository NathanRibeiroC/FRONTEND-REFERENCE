# REACT-REFERENCE
This repository is a personal reference for React concepts and snippets of code that are used daily.

:page_facing_up: 
<ul>
  <li>MAIN DOCUMENTATION CONCEPTS</li>
</ul>

#### MAIN DOCUMENTATION CONCEPTS (CLASS COMPONENTS)

React elements are immutable. Once you create an element, you can’t change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

The simplest way to define a component is to write a JavaScript function:

```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
