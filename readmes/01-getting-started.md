- ## [1 Understanding single and multi page applications](#1_understanding_single_and_multi_page_applications)

# <a name="1_understanding_single_and_multi_page_applications"></a>1 Understanding single and multi page applications

![single-&-multipage-apps](./images/single-&-multipage-apps.png)

---

Components are the **core building block of React apps**. Actually, React really is just a library for creating components in its core.

A typical React app therefore could be depicted as a **component tree** - having one root component ("App") and then an potentially infinite amount of nested child components.

Each component needs to return/ render some **JSX** code - it defines which HTML code React should render to the real DOM in the end.

**JSX is NOT HTML** but it looks a lot like it. Differences can be seen when looking closely though (for example className in JSX vs class in "normal HTML"). JSX is just syntactic sugar for JavaScript, allowing you to write HTMLish code instead of nested React.createElement(...) calls.

When creating components, you have the choice between two different ways:

1. Functional components (also referred to as "presentational", "dumb" or "stateless" components. ES6 arrow functions as shown here is recommended but optional.

```js
const cmp = () => {
  return <div>some JSX</div>;
};
```

2. Class-based components (also referred to as "containers", "smart" or "stateful" components)

```js
class Cmp extends Component {
  render() {
    return <div>some JSX</div>;
  }
}
```

Note that you should use 1) as often as possible though. It's the best-practice.

---

# this is a test again
