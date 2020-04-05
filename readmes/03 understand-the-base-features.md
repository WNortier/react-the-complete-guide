# Understanding the Base Features & Syntax

- ## [The Build Workflow](#The_Build_Workflow)
- ## [Using Create React App](#Using_Create_React_App)
- ## [Understanding Folder Structure](#Understanding_Folder_Structure)
- ## [Component Basics](#Component_Basics)

- ## [JSX & JSX Restrictions](#JSX_&_JSX_Restrictions)
- ## [Creating a Functional Component](#Creating_a_Functional_Component)
- ## [Outputting Dynamic Content in Components](#Outputting_Dynamic_Content_in_Components)
- ## [Working with `props` and `props.children`](#Working_with_Props)


# <a name="The_Build_Workflow"></a> The Build Workflow

![build-workflow](./images/03-base-features/build-workflow.png)

# <a name="Using_Create_React_App"></a> Using Create React App

`npm install create-react-app -g`

`create-react-app react-complete-guide`

`create-react-app react-complete-guide--scripts-version 1.1.5`

    npm start

Launches dev server

# <a name="Understanding_Folder_Structure"></a> Understanding Folder Structure

# <a name="Component_Basics"></a> Component Basics

React is all about making components.  In the App.js file we see the primary react component.

It is a Javascript class named App which inherits from the imported Component class from the react library.  Additionally we must always import React into our components.  Every React component has to return some html code which can be rendered to the DOM.  This is actually known as JSX - not html and its the reason for importing React to compile the JSX.   

The component is then exported as the default export.
App.js: 
```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h2>Welcome to React</h2>
        </div>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
```
This App component is imported into the index.js file where it gets rendered into the place of the root element.  Inside of this app component we will nest all the other components our application might need.  We can also nest other components into eachother but there will always only be one root App component.  

Index.js:
```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(<App />, document.getElementById('root')
);
```

# <a name="JSX_&_JSX_Restrictions"></a> JSX & JSX Restrictions

JSX is different from regular html.  For instance, we cannot use the word class we need to use **className**. It is also a convention to wrap everything in a single div.  JSX is just syntactic sugar for JavaScript, allowing you to write HTMLish code instead of nested `React.createElement(...)` calls. 

App.js
```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (                                                                // Notice parenthesis 
      <div className="App">                                                 // Wrapping div
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />                // className 
          <h2>Welcome to React</h2>
        </div>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );                                                                      // Notice parenthesis
  }
}

export default App;
```
# <a name="Creating_a_Functional_Component"></a> Creating a Functional Component

When creating components, you have the choice between two different ways:
## 1. Functional components (using ES6 arrow functions as shown here is recommended but optional)
```js
const cmp = () => { 
    return <div>some JSX</div> 
} 
```
## 2. Class-based components 
```js
class Cmp extends Component { 
    render () {
        return <div>some JSX</div> 
    } 
}
```
In React its a convention to create **folder names for components with starting capital letters** along with **component file names which match this name**. 

Most of the time the best way to create a component is by creating a simple functional component.  
Remember that to return our JSX we need to import react from the react package.  

Person folder -> Person.js
```js
import React from 'react';

const person = () => {
    return <h1>I'm a person</h1>
}

export default person;
```

Now in the App.js file we import our person component and **components always need to start with an Uppercase character** so as to not interfere with the JSX.  
We can either import it with a closing tag `<Person></Person>` or since we have nothing in between a single tag `<Person />`. 
We can import the component multiple times but copy pasting the component.

App.js
```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (                                                                 
      <div className="App">
          <h2>I'm a React App</h2>
          <Person />
      </div>
    );                                                                      
  }
}

export default App;
```
# <a name="Ouputting_Dynamic_Content_in_Components"></a> Outputting Dynamic Content in Components

Al though we can't define a Javascript class in our JSX we can define short simple expressions such as calculations or function calls which could do more complex stuff. 

Person folder -> Person.js
```js
import React from 'react';

const person = () => {
    return <h1>I'm a person who is {Math.floor(Math.random() * 30)} years old.</h1>
}

export default person;
```

# <a name="Working_with_Props"></a> Working with `props` and `props.children'

Its possible to pass dynamic content to components from outside as props.
> Note that we can also pass **complex `html` content** between the opening and closing tags of our components.  

App.js
```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (                                                                 
      <div className="App">
          <h2>I'm a React App</h2>
          <Person name="Leon" age="58"/>
          <Person name="Warwick" age="29">Hobbies: coding</Person>
      </div>
    );                                                                      
  }
}

export default App;
```

In our component `props` is passed as a parameter where `props.children` refers to the html content passed in between our component tags in the parent.  Since we are passing more content now we will **add opening and closing brackets** to our return statement and wrap our code in a div.   

Person folder -> Person.js
```js
import React from 'react';

const person = (props) => {
    return (
      <div>
      <h1>I'm a person my name is {props.name} and I am {props.age} years old.</h1>
      <h1>{props.children}</h1>
      </div>
    )
}

export default person;
```