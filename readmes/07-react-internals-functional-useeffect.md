---
<a name="Back_To_Top"></a> Top
---

- ### [Using UseEffect in Functional Components](#Using_UseEffect_in_Functional_Components)
- ### [Controlling the useEffect() Behaviour](<#Controlling_the_useEffect()_Behaviour>)
- ### [Cleaning Up with UseEffect()](<#Cleaning_Up_with_UseEffect()>)

---

## <a name="Using_UseEffect_in_Functional_Components"></a>Using UseEffect in Functional Components

If we're using the right React version that actually supports React hooks there is an equivalent for the lifecycle hooks that class based components have. We can of course manage state with `useState({})` but this is not it - there is another hook we can import which is called `useEffect()`

> ### `useEffect()` basically combines the functionality or the use cases you can cover of all the class-based lifecycle hooks but for functional components.

Remember that al though both are called hook they are not related. This is not a lifecycle hook - this is basically the second most important React hook after `useState({})`. `useEffect()` is basically a function you can add anywhere into one of your functional component bodies. By default `useEffect()` takes a function without arguments which will run for every render cycle of the component it is added to.

If no performance optimisations are added the render method of `App.js` will cause the render cycle of all its included components to be triggered. As it comes it is component did mount and component did update in one effect.

I say re-rendered, I don't mean in the real DOM as you will learn but in that virtual DOM, React will check if it needs to touch the real DOM. We can prevent this and we'll do so later but for now, it is what it is.

> ### Note that some hooks like `getDerivedStateFromProps(nextProps, nextState)` are not included but you also don't really need that because if you have `props` here and you want to base your state on that, well then you can use `useState({})` and pass some data from your props as an initial state into this.

**components -> Cockpit > Cockpit.js**

```js
import React, { useEffect } from 'react';
import classes from './Cockpit.css';

const cockpit = (props) => {
  useEffect(() => {
    console.log('[Cockpit.js] useEffect');
  });

  const assignedClasses = [];
  let btnClass = '';
  if (props.showPersons) {
    btnClass = classes.Red;
  }

  if (props.persons.length <= 2) {
    assignedClasses.push(classes.red); // classes = ['red']
  }
  if (props.persons.length <= 1) {
    assignedClasses.push(classes.bold); // classes = ['red', 'bold']
  }

  return (
    <div className={classes.Cockpit}>
      <h1>{props.title}</h1>
      <p className={assignedClasses.join(' ')}>This is really working!</p>
      <button className={btnClass} onClick={props.clicked}>
        Toggle Persons
      </button>
    </div>
  );
};

export default cockpit;
```

---

- [Top](#Back_To_Top)

---

## <a name="Controlling_the_useEffect()_Behaviour"></a>Controlling the useEffect() Behaviour

`useEffect(()=>{})` can be tricky to use though because right now, it runs all the time and combines `componentDidMount()` and `componentDidUpdate()` into one.

What if we wanted to send an HTTP request here only when the component is rendered for the first time and not for every re-render cycle? Lets add a `setTimeout(() => {alert('fetched some data')}, 3000)` to fake an HTTP request.

### 1. To control when this hook executes, such as when our `Persons` component changes and not under any other condition we can add a second argument to `useEffect()`. That second argument is an array that second argument is an array where you simply point at all the variables or all the data that actually are used in your effect.

If we know, we only want to run this code when our persons changed, well then you simply add `props.persons` here. Now by the way if you have different effects that depend on different data, you can use useEffect more than once, that is absolutely fine. Y**_ou can also pass multiple fields you depend on to the array._**

### 2. If we now only want to execute this when the component renders the first time there is a little shortcut so to say - you can pass an empty array. So if you just need `componentDidMount()` you can use `useEffect()` with an empty array as the second argument.

```js
import React, { useEffect } from 'react';
import classes from './Cockpit.css';

const cockpit = (props) => {
  useEffect(() => {
    console.log('[Cockpit.js] useEffect');
    setTimeout(() => {
      alert('fetched data from server');
    });
    // Specifying that our useEffect() should only execute when our persons change by passing an array with props.persons as the second argument
  }, [props.persons, b, c]);

  // Remember that having more than one useEffect() hook is totally legal
  // useEffect(() => {})

  const assignedClasses = [];
  let btnClass = '';
  if (props.showPersons) {
    btnClass = classes.Red;
  }

  if (props.persons.length <= 2) {
    assignedClasses.push(classes.red); // classes = ['red']
  }
  if (props.persons.length <= 1) {
    assignedClasses.push(classes.bold); // classes = ['red', 'bold']
  }

  return (
    <div className={classes.Cockpit}>
      <h1>{props.title}</h1>
      <p className={assignedClasses.join(' ')}>This is really working!</p>
      <button className={btnClass} onClick={props.clicked}>
        Toggle Persons
      </button>
    </div>
  );
};

export default cockpit;
```

## <a name="Cleaning_Up_with_UseEffect()"></a>Cleaning Up with UseEffect()

You can also use `useEffect()` for cleanup work. This hook runs **before** the main useEffect function runs, but **after** the (first) render cycle.

In your function you pass to your effect, so in this anonymous arrow function here in my case, you can return either nothing, so you don't have to have a return statement but if you add one, you can return a function here that will run after every render cycle.

```js
import React, { useEffect } from 'react';
import classes from './Cockpit.css';

const cockpit = (props) => {
  useEffect(() => {
    console.log('[Cockpit.js] useEffect');
    setTimeout(() => {
      alert('fetched data from server');
    });
    // Specifying that our useEffect() should only execute when our persons change by passing an array with props.persons as the second argument
  }, [props.persons, b, c]);

  // Remember that having more than one useEffect() hook is totally legal
  // useEffect(() => {})

  const assignedClasses = [];
  let btnClass = '';
  if (props.showPersons) {
    btnClass = classes.Red;
  }

  if (props.persons.length <= 2) {
    assignedClasses.push(classes.red); // classes = ['red']
  }
  if (props.persons.length <= 1) {
    assignedClasses.push(classes.bold); // classes = ['red', 'bold']
  }

  return (
    <div className={classes.Cockpit}>
      <h1>{props.title}</h1>
      <p className={assignedClasses.join(' ')}>This is really working!</p>
      <button className={btnClass} onClick={props.clicked}>
        Toggle Persons
      </button>
    </div>
  );
};

export default cockpit;
```

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

![single-&-multipage-apps](./images/next-gen/imports-exports-2.png)

[Table Lookups -> nwId](https://github.com/WNortier/nextworld/blob/master/nextworld-platform-tutorials/01-build-an-application/00-build-an-application-overview.md#3_TABLE_LOOKUPS)
