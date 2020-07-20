---
<a name="Back_To_Top"></a> Top
---

- ### [How React Updates the DOM](#How React Updates the DOM)

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

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

![single-&-multipage-apps](./images/next-gen/imports-exports-2.png)

[Table Lookups -> nwId](https://github.com/WNortier/nextworld/blob/master/nextworld-platform-tutorials/01-build-an-application/00-build-an-application-overview.md#3_TABLE_LOOKUPS)
