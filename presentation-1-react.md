autoscale: true

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

# Handling Input

Let's keep running with this example. let's say we can click on an article to mark it as "read".

We update our state in `ArticleList` to have a `read` boolean.

```js
class ArticleList extends Component {
  constructor() {
    this.state = {
      articles = [
        { id: 'a', title: 'A Fascinating Look At Shoes', read: false },
        { id: 'b', title: 'A Quick Word from Smelly Pete', read: false },
        { id: 'c', title: 'The Weather: It sucks', read: false }
      ]
    }
  }
  // ...
}
```

---

To mark an article as 'read', you might try something like this:

```js
class Article extends Component {
  markAsRead() {
    this.props.read = true;
  }

  render() {
    return (
      <div className="article" onClick={this.markAsRead}>
        <h2>{ this.props.title }</h2>
      </div>
    );
  }
}
```

The problem, though, is that a component *cannot modify its props*.

---

# State Vs. Props

The `<Article>` component gets its data from "props" - the "properties" provided.

Props are passed down from the parent, and they are immutable inside the component.

`<ArticleList>` gets its data from "state".

State is a reactive data source that causes re-renders when it changes.

---

# The right way to do it

```js
class ArticleList extends Component {
  constructor() { /* Stuff here */ }

  markAsRead(articleId) {
    let articleIndex = this.state.articles.findIndex( a => a.id === articleId );
    let newArticles  = this.state.articles.slice();
    newArticles[articleIndex].read = true;

    this.setState({ articles: newArticles });
  }

  render() { /* Stuff here */ }

  renderArticle(article) {
    return <Article title={article.title} key={article.id} handleClick={this.markAsRead} />;
  }
}
```

---

```js
class Article extends Component {
  render() {
    return (
      <div className="article" onClick={ () => this.props.markAsRead(this.props.article.id) }>
        <h2>{ this.props.title }</h2>
      </div>
    );
  }
}

```

---

# The Benefit of One-Way Data Flow
All of your logic to change a piece of state lives in *one place*.

```js
class ArticleList extends Component {
  markAsRead(articleId) { /* Stuff */ }

  leaveComment(articleId, commentText) { /* Stuff */ }

  setFavourite(articleId) { /* Stuff */ }

  reportSpam(articleId) { /* Stuff */ }

  share(articleId, shareMethod) { /* Stuff */ }
}
```

---

# *Redux* makes this more manageable

---

Redux keeps all of your application's state in a single immutable object.

To change the state, you dispatch an *action* that describes the change.

Functions called *reducers* create the new state based on the old state + the action.

This state then gets passed down through your components for React to do its thing.

---

It also
