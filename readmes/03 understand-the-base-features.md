# Understanding the Base Features & Syntax

- ## [The Build Workflow](#The_Build_Workflow)
- ## [Using Create React App](#Using_Create_React_App)
- ## [Understanding Folder Structure](#Understanding_Folder_Structure)
- ## [Component Basics](#Component_Basics)

- ## [JSX & JSX Restrictions](#JSX & JSX Restrictions)
- ## [6 Garbage Collection](#6_Garbage_Collection)

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

# <a name="JSX & JSX Restrictions"></a> JSX & JSX Restrictions

JSX is different from regular html.  For instance, we cannot use the word class we need to use **className**. It is also a convention to wrap everything in a single div. 

```javascript
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">                                                 // Wrapping div
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />                // className 
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
# <a name="6_Garbage_Collection"></a> 6 Garbage Collection



