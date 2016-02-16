![inline](https://raw.githubusercontent.com/wiki/facebook/react/react-logo-1000-transparent.png)

# Introduction to React.js
### By Josh

---

# [fit] What _is_ React.js?

React lists 3 primary defining characteristics:

* Just the UI
* Virtual DOM
* Data Flow

---

# 1. Just the UI

React is not an MVC framework.
It's a *portable UI framework*
that can be used in a bunch of different ways*

\* More on this later

---

# 2. Virtual DOM

To understand the Virtual DOM, we have to understand a little bit about how React works.

The virtual DOM is an implementation detail for a very interesting philosophy.

---

React's *true power* is in computing changes in state.

You provide React with your application's state, and it works out what needs to change for the UI to match.

This is *declarative*. You tell it what you want it to display, not how to display it.

---

![fit](http://i.imgur.com/UI7y1jX.png)

---

# How does it reconcile?

This declarative business sounds great, but surely it's crazy slow, right?

React has to parse the entire DOM whenever anything changes to see what needs to be updated!

Thankfully, React has a smarter solution.

---

The Virtual DOM is just a javascript object. It only holds the necessary data.

Javascript can parse this data structure _much faster_ than it can parse the DOM.

It also uses a bunch of interesting techniques to further optimize.

---

# 3. Data Flow

Most modern frameworks like Angular and Ember use _data binding_ to connect things.

React takes a different approach: It wants data to flow through components.

When the underlying data changes, the UI changes, but the reverse is not automatically true.

---

# A first look at a React component.

```js
class Article extends Component {
  render() {
    return (
      <div className="article">
        <h2>{ this.props.title }</h2>
      </div>
    );
  }
}
```

---

```js
class ArticleList extends Component {
  constructor() {
    this.state = {
      articles: [
        { id: 'a', title: 'A Fascinating Look At Shoes' },
        { id: 'b', title: 'A Quick Word from Smelly Pete' },
        { id: 'c', title: 'The Weather: It sucks' }
      ]
    }
  }

  render() {
    return (
      <div id="article-list">
        { this.state.articles.map( this.renderArticle )}
      </div>
    );
  }

  renderArticle(article) {
    return <Article title={article.title} key={article.id} />;
  }
}
```

---

# State Vs. Props

The `<Article>` component gets its data from its "props".

Props are passed down from the parent, and they are immutable inside the component.

`<ArticleList>` gets its data from "state".

State is a reactive data source that causes re-renders when it changes.
