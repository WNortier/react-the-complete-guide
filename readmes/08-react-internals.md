---
<a name="Back_To_Top"></a> Top
---

- ### [How React Updates the DOM](#How React Updates the DOM)
- ### [Rendering adjacent JSX Elements](#Rendering_adjacent_JSX_Elements)

---

## <a name="How_React_Updates_the_DOM"></a>How React Updates the DOM

![diving deeper](./images/06-diving-deeper/virtual-dom.png)

- The `render()` method can be misleading, this does not mean that it renders it to the DOM. Render is more a suggestion of what the HTML should look like in the end but render can very well be called and lead to the same result as is already displayed and that is part of the reason why we use shouldComponentUpdate to prevent unnecessary render calls.

- Even if we don't catch an unnecessary render call, maybe a prop did change and still we would render the same result for whatever reason, even then this does not mean that it immediately hits the real DOM and starts re-rendering it, instead it first of all does something else, it compares virtual DOMs, It has an old virtual DOM and a re-rendered or a future virtual DOM. _React takes this virtual DOM approach because it's faster than the real DOM._ Now a virtual DOM simply is a DOM representation in Javascript.

- It has the old virtual DOM and then the re-rendered one, the re-rendered one is the one which gets created when the render method is called. Now as I mentioned though, re-rendering or calling render doesn't immediately update the real DOM, instead React makes a comparison. It compares the old virtual DOM to the new one and it checks if there are any differences.

- It compares the old virtual DOM to the new one and it checks if there are any differences. If it can detect differences, it reaches out to the real DOM and updates it and even then, it doesn't re-render the real DOM entirely, it only changes it in the places where differences were detected, for example if a button text changed, it will only update that text and not re-render the whole button.

- If no differences were found, then it doesn't touch the real DOM. Render did execute, the comparison was made and that is why `shouldComponentUpdate()` might make sense to prevent this.

> ### Accessing the DOM is really slow, this is something you want to do as little as possible and hence, React has this virtual DOM idea where it compares the virtual DOM and makes sure that the real DOM is only touched if needed.

---

- [Top](#Back_To_Top)

---

## <a name="Rendering_adjacent_JSX_Elements"></a>Rendering adjacent JSX Elements

You must only have one root JSX element, be that another component or a normal HTML element.

Now technically, an array of course still is one object but with multiple elements in there and indeed, React does allow us to return an array of adjacent elements _as long as all the items in there have a key_ and that key is required so that React can efficiently update and reorder these elements as it might be required by your app.

### Solution 1 [Array of JSX with a key]

I will return my JSX elements in an array separated by commas. Now this might look strange but remember, JSX is just syntactic sugar for React create element.
We can simply add a key on these elements and here we're not generating the key dynamically, we're not extracting a unique value from anywhere.

**Components -> Persons -> Person -> Person.js**

```js
import React, { Component } from "react";
import classes from "./Person.css";

class Person extends Component {
  render() {
    console.log("[Person.js] rendering...");
    return [
      <p key="i1" onClick={this.props.click}>
        I'm {this.props.name} and I am {this.props.age} years old!
      </p>,
      <p key="i2">{this.props.children}</p>,
      <input
        key="i3"
        type="text"
        onChange={this.props.changed}
        value={this.props.name}
      />,
    ];
  }
}

export default Person;
```

> ### it's important to know that if you don't technically need a wrapping element for a styling or structural reasons, then you can avoid it by using an array.

### Solution 2 [Wrapping component]

You can create a wrapping component that does not render any actual HTML code but that simply is there to fulfill React's requirement of having a wrapping component.

I'll create a new folder which I'll name hoc which stands for a higher order component named `Aux` - this will need to be named `Auxilliary` on windows since `Aux` is a reserved word.

`children` is a special property that simply outputs whatever gets entered between the opening and closing tag of this component.

**Components -> hoc -> Aux.js**

```js
const aux = (props) => props.children;

export default aux;
```

It's basically an empty wrapper using that special children property which React reserves for us and children will always refer to the content between the opening and closing tag of your component.

**Components -> Persons -> Person -> Person.js**

```js
import React, { Component } from "react";

import Aux from "../../../hoc/Aux";
import classes from "./Person.css";

class Person extends Component {
  render() {
    console.log("[Person.js] rendering...");
    return (
      <Aux>
        <p onClick={this.props.click}>
          I'm {this.props.name} and I am {this.props.age} years old!
        </p>
        <p>{this.props.children}</p>
        <input
          type="text"
          onChange={this.props.changed}
          value={this.props.name}
        />
      </Aux>
    );
  }
}

export default Person;
```

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

![single-&-multipage-apps](./images/next-gen/imports-exports-2.png)

[Table Lookups -> nwId](https://github.com/WNortier/nextworld/blob/master/nextworld-platform-tutorials/01-build-an-application/00-build-an-application-overview.md#3_TABLE_LOOKUPS)
