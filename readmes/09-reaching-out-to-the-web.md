---
<a name="Back_To_Top"></a> Top
---

- ### [Understanding Http Requests in React](#Understanding_Http_Requests_in_React)
- ### [Introducing Axios](#Introducing_Axios)
- ### [Creating a Http Request to GET Data](#Creating_a_Http_Request_to_GET_Data)
- ### [Fetching Data on Update without Creating Infinite Loops](#Fetching_Data_on_Update_without_Creating_Infinite_Loops)
- ### [POSTing Data to the Server](#POSTing_Data_to_the_Server)
- ### [Handling Errors Locally with Axios](#Handling_Errors_Locally_with_Axios)
- ### [Adding Interceptors to Execute Code Globally](#Adding_Interceptors_to_Execute_Code_Globally)

---

## <a name="Understanding_Http_Requests_in_React"></a>Understanding Http Requests in React

So how does sending HTTP requests work in react applications typically and I'm saying typically because if you're creating a multi-page react application, there actually isn't anything special to it.

#### SPA vs Non-SPA

- You probably have your application, your page where you submit a form and that sends a request to your backend and you get back a new html page with potentially new react code in there. You don't have a single page application, you don't have a strong decoupling going on.

- If you have such a **single page application**, you have a strong differentiation between your frontend, your react app and your backend though. The react app and the servers still need to communicate from time to time but they don't communicate by exchanging html pages. So if a react app sends a request to a server, you don't get back a new html page instead you can back some `json` data typically or you send some json data to the server if you want to create some resources on the server for example.

> ### This is how that works, you have that decoupling and your server therefore typically is a RESTful API, just exposing some API endpoints to which you can send requests

![single-&-multipage-apps](./images/reaching-out/01.png)

---

- [Top](#Back_To_Top)

---

## <a name="Introducing_Axios"></a>Introducing Axios

https://www.npmjs.com/package/axios

`npm install --save axios`

Lets explore the basic crud operations using dummy data from jsonplaceholder as our 'server' endpoint: https://jsonplaceholder.typicode.com/

#### Option 1 Axios

Axios is a third-party Javascript library which you can add to any Javascript project, it's not connected to React at all but of course it fits nicely into Raect because it's Javascript

#### Option 2 `XMLHttpRequest`

You have basically two options, Javascript of course has the `XMLHttpRequest` object. With that you can construct your own Ajax requests and send them to a specific URL and handle the response. Nothing wrong with that since React is just about writing Javascript everywhere, you can of course use all the Javascript features including XML HTTP request. But writing and configuring requests with that object manually is quite cumbersome

---

- [Top](#Back_To_Top)

---

## <a name=Creating_a_Http_Request_to_GET_Data"></a>Creating a Http Request to GET Data

Now where do we make this HTTP request then?

If we have a look at the lifecycle hooks we encountered during component creation, there is one life cycle hook we should use for side effects, `componentDidMount()`. The HTTP request is a side effect, it doesn't affect your react logic or something like that but it has the side effect of fetching new data and if your react application is dynamically outputting some data which it probably is, the data changing of course is a side effect affecting your application.

![single-&-multipage-apps](./images/reaching-out/02.png)

> ### So componentDidMount is a great place for causing side effects but not for updating state since it triggers a re-render.

We will still update the state here once the HTTP request has gone and got us new data because we actually want to re-update the page.

```js
class Blog extends Component {
  state = {
    posts: [],
    selectedPostId: null,
    error: false,
  };

  componentDidMount() {
    // WE CAN'T STORE THE RESPONSE IN A CONSTANT LIKE THIS SINCE ITS ASYNCHRONOUS DATA AND WILL TAKE SOME TIME BEFORE WE RECEIVE IT,
    // TOO LONG FOR THE DATA TO BE SAVED IN THE CONST
    // const data = (won't work)
    axios
      .get("/posts")
      .then((response) => {
        // REDUCING THE SIZE OF THE ARRAY / DATA WE GOT BACK USING SLICE
        const posts = response.data.slice(0, 4);
        // CLEVER CODE WHICH ADDS AN ADDITIONAL ATTRIBUTE/VALUE TO AN EXISTING OBJECT
        const updatedPosts = posts.map((post) => {
          return {
            ...post,
            author: "Max",
          };
        });
        this.setState({ posts: updatedPosts });
        // console.log( response );
      })
      .catch((error) => {
        // console.log(error);
        this.setState({ error: true });
      });
  }

  postSelectedHandler = (id) => {
    this.setState({ selectedPostId: id });
  };

  render() {
    let posts = <p style={{ textAlign: "center" }}>Something went wrong!</p>;
    if (!this.state.error) {
      posts = this.state.posts.map((post) => {
        // PASSING IN THE POST DATA WE GOT BACK FROM OUR GET REQUEST
        return (
          <Post
            key={post.id}
            title={post.title}
            author={post.author}
            clicked={() => this.postSelectedHandler(post.id)}
          />
        );
      });
    }

    // FULLPOST COMPONENT DISPLAYS ALL THE DATA ABOUT THE POST IF A CARD IS CLICKED IN THE POST COMPONENT
    //  NEWPOST HAS SETTINGS FOR ADDING A NEW POST
    return (
      <div>
        <section className="Posts">{posts}</section>
        <section>
          <FullPost id={this.state.selectedPostId} />
        </section>
        <section>
          <NewPost />
        </section>
      </div>
    );
  }
}

export default Blog;
```

> ### Axios therefore uses promises, a default javascript object introduced with ES6 and thanks to our workflow we're using with create react app also available in older browsers since the code gets compiled to code which also works in older browsers.

---

- [Top](#Back_To_Top)

---

## <a name=">Fetching_Data_on_Update_without_Creating_Infinite_Loops"></a>Fetching Data on Update without Creating Infinite Loops

Let's have a look. If we have a look at the lifecycle hook for updating components and we are of course speaking about the update lifecycle hook because the component is there right from the start but data should be fetched once we receive a new prop ID.

> ### Then componentDidUpdate is a good place for causing side effects, it also has one issue though. If we update the state in there, we update the component again and we therefore enter an infinite loop, that is something we'll have to watch out for.

Notice there is also a `delete` post handler which is uses the DELETE method

> ### The Delete method also takes a URL and we need to target a specific post with it using the `delete` method.

```js
import React, { Component } from "react";
import axios from "axios";

import "./FullPost.css";

class FullPost extends Component {
  state = {
    loadedPost: null,
  };

  componentDidUpdate() {
    if (this.props.id) {
      //   THESE IF CHECKS ARE PREVENTING AN INFINITE LOOP WHICH WOULD OTHERWISE BE CAUSED BY this.setState()
      if (
        !this.state.loadedPost ||
        (this.state.loadedPost && this.state.loadedPost.id !== this.props.id)
      ) {
        axios.get("/posts/" + this.props.id).then((response) => {
          // console.log(response);
          this.setState({ loadedPost: response.data });
        });
      }
    }
  }

  deletePostHandler = () => {
    axios.delete("/posts/" + this.props.id).then((response) => {
      console.log(response);
    });
  };

  render() {
    let post = <p style={{ textAlign: "center" }}>Please select a Post!</p>;
    //  IF WE HAVE THE ID PROP WE CAN SHOW SOME LOADING TEXT
    if (this.props.id) {
      post = <p style={{ textAlign: "center" }}>Loading...!</p>;
    }
    //  ONLY SHOW THE LOADED POST ONCE THE ASYNCHRONOUS HAS RETURNED DATA
    if (this.state.loadedPost) {
      post = (
        <div className="FullPost">
          <h1>{this.state.loadedPost.title}</h1>
          <p>{this.state.loadedPost.body}</p>
          <div className="Edit">
            <button onClick={this.deletePostHandler} className="Delete">
              Delete
            </button>
          </div>
        </div>
      );
    }
    return post;
  }
}

export default FullPost;
```

---

- [Top](#Back_To_Top)

---

## <a name="POSTing_Data_to_the_Server"></a>POSTing Data to the Server

Lets look at how you can post data to a server.

> ### Now a post request needs more than the URL though, it needs a second argument which is the data we want to send because obviously a post request without data doesn't make that much sense.

The data we are passing is of course a javascript object but axios will automatically basically stringify this, so turn this into json data, it will do it for us just as it extract the json data and get requests for us too.

```js
import React, { Component } from "react";
import axios from "axios";

import "./NewPost.css";

class NewPost extends Component {
  state = {
    title: "",
    content: "",
    author: "Max",
  };

  postDataHandler = () => {
    const data = {
      title: this.state.title,
      body: this.state.content,
      author: this.state.author,
    };
    axios.post("/posts", data).then((response) => {
      console.log(response);
    });
  };

  render() {
    return (
      <div className="NewPost">
        <h1>Add a Post</h1>
        <label>Title</label>
        <input
          type="text"
          value={this.state.title}
          onChange={(event) => this.setState({ title: event.target.value })}
        />
        <label>Content</label>
        <textarea
          rows="4"
          value={this.state.content}
          onChange={(event) => this.setState({ content: event.target.value })}
        />
        <label>Author</label>
        <select
          value={this.state.author}
          onChange={(event) => this.setState({ author: event.target.value })}
        >
          <option value="Max">Max</option>
          <option value="Manu">Manu</option>
        </select>
        <button onClick={this.postDataHandler}>Add Post</button>
      </div>
    );
  }
}

export default NewPost;
```

---

- [Top](#Back_To_Top)

---

## <a name="Handling_Errors_Locally_with_Axios"></a>Handling Errors Locally with Axios

Of course you can do more than logging the error, you can update your UI, you can update the state to show something went wrong, let me show this. Here we can set an `error` property, which is set to `false` initially and in that `catch` method here, I don't want to log the error, instead I'll `setState({})` and I'll set error to `true`.

Now what we can do is we can render something different to the screen using this error `boolean`.

```js
class Blog extends Component {
  state = {
    posts: [],
    selectedPostId: null,
    error: false,
  };

  componentDidMount() {
    axios
      // URL ERROR
      .get("/postsfdsfds")
      .then((response) => {
        const posts = response.data.slice(0, 4);
        const updatedPosts = posts.map((post) => {
          return {
            ...post,
            author: "Max",
          };
        });
        this.setState({ posts: updatedPosts });
        // console.log( response );
      })
      // USING THE CATCH BLOCK TO CATCH POTENTIAL ERRORS
      .catch((error) => {
        // console.log(error);
        this.setState({ error: true });
      });
  }
  postSelectedHandler = (id) => {
    this.setState({ selectedPostId: id });
  };

  //
  render() {
    // POSTS IS INITIALLY SET TO AN ERROR MESSAGE, BUT IF THE ERROR BOOLEAN IS FALSE IT WON'T SHOW
    let posts = <p style={{ textAlign: "center" }}>Something went wrong!</p>;
    if (!this.state.error) {
      posts = this.state.posts.map((post) => {
        // PASSING IN THE POST DATA WE GOT BACK FROM OUR GET REQUEST
        return (
          <Post
            key={post.id}
            title={post.title}
            author={post.author}
            clicked={() => this.postSelectedHandler(post.id)}
          />
        );
      });
    }
    return (
      <div>
        <section className="Posts">{posts}</section>
        <section>
          <FullPost id={this.state.selectedPostId} />
        </section>
        <section>
          <NewPost />
        </section>
      </div>
    );
  }
}

export default Blog;
```

---

- [Top](#Back_To_Top)

---

## <a name="Adding_Interceptors_to_Execute_Code_Globally"></a>Adding Interceptors to Execute Code Globally

Sometimes in some places, you maybe want to execute some code globally. So no matter which HTTP request you send, from within which component, you want to do something when that request gets sent and or when the response returns.

> ### You can do it with axios with the help of so-called interceptors, these are functions you can define globally which will be executed for every request leaving your app and every response returning into it.

This is especially useful for example for setting some common headers like authorization header maybe or for responses if you want to log responses or want to handle errors globally.

I now also want to import axios from the axios package and **all these axios imports actually all share the same configuration** which is why we set it up in `index.js`.

---

### 1 `axios.interceptors.request.use`

We can access the `request` object and now I'll simply add `use` to register a new interceptor, that interceptor takes a function as an input which receives the config or the request you could say. Now let's simply log that request to the console for now so that we can see what's inside of it.

`console.log(request);` yields the following in the console, simply our request with all its parts:

![single-&-multipage-apps](./images/reaching-out/03.png)

> ### In your interceptor function here, you need to always return the request or the request config otherwise you're blocking the request.

---

### 2 `axios.interceptors.response.use`

The reason for this is that this error here is related to sending the request,

for example if you had no internet connectivity or something like that, it should pop up

so if the request sending fails. We can also add an interceptor to handle responses though, we do in the

same way as you do it for the request but we use the response object on the interceptors object.

**index.js**

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import registerServiceWorker from "./registerServiceWorker";
import axios from "axios";

axios.defaults.baseURL = "https://jsonplaceholder.typicode.com";
axios.defaults.headers.common["Authorization"] = "AUTH TOKEN";
axios.defaults.headers.post["Content-Type"] = "application/json";

axios.interceptors.request.use(
  (request) => {
    console.log(request);
    // Edit request config
    return request;
  },
  (error) => {
    console.log(error);
    return Promise.reject(error);
  }
);

axios.interceptors.response.use(
  (response) => {
    console.log(response);
    // Edit request config
    return response;
  },
  (error) => {
    console.log(error);
    return Promise.reject(error);
  }
);

ReactDOM.render(<App />, document.getElementById("root"));
registerServiceWorker();
```

Now for that interceptor here, we can also pass a second function besides that request configuration changing function, we can add a function which handles any errors. So here we can log an error like this, we should also return `Promise.reject(error)` here though so that we still forward it to our request as we wrote it in a component where we can handle it again with the `catch` method. This make sense if you have some local task you want to do like show something on the UI but also globally, you want to log it in the log file which you send to a server or something like that.

---

- [Top](#Back_To_Top)

---

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

```js

```

---

- [Top](#Back_To_Top)

---

- ### [1 TEMPLATE](#1_TEMPLATE)

## <a name="1_TEMPLATE"></a>1 TEMPLATE

```js

```

---

- [Top](#Back_To_Top)

---
