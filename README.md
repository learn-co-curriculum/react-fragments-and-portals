# React Fragments

## Objectives

- Explain the use of fragments in React components

## Why Fragments

It is required that every React component must return a single JSX element.
Because of this, we often use the HTML-like elements such as `div` to wrap other
elements within the JSX. When rendered, this creates a DOM element for that
outer `div`, which is sometimes unnecessary. For example:

```jsx
class ChildComponent extends Component {
  render() {
    return (
      <div>
        <p>Hey, I am a child</p>
        <p>My name is child component</p>
      </div>
    );
  }
}

class ParentComponent extends Component {
  render() {
    return (
      <div className="parent">
        <ChildComponent />
        <ChildComponent />
      </div>
    );
  }
}
```

This set up creates a DOM structure that looks like this:

```html
<div class="parent">
  <div>
    <p>Hey, I am a child</p>
    <p>My name is child component</p>
  </div>
  <div>
    <p>Hey, I am a child</p>
    <p>My name is child component</p>
  </div>
</div>
```

Those nested `div`s don't have any purpose here and don't have any styling
besides their default properties. Without them though, we would have an error as
there are _two_ `p` tags being returned in the ChildComponent. Instead, we could
use JSX Fragments, preventing the extra `div`s from being added to the DOM:

```jsx
class ChildComponent extends Component {
  render() {
    //The wrapping 'div' here has been replaced with a React fragment
    return (
      <>
        <p>Hey, I am a child</p>
        <p>My name is child component</p>
      </>
    );
  }
}

class ParentComponent extends Component {
  render() {
    return (
      <div>
        <ChildComponent />
        <ChildComponent />
      </div>
    );
  }
}
```

With the fragment in place, the DOM will now look like this:

```html
<div>
  <p>Hey, I am a child</p>
  <p>My name is child component</p>
  <p>Hey, I am a child</p>
  <p>My name is child component</p>
</div>
```

The `<>` and `</>` are shorthand for `<React.Fragment>` and `</React.Fragment>`
and can be used interchangeably. **They allow a component to return multiple
elements without adding a wrapper element that adds to the DOM.**

Fragments are not restricted to the outermost element being returned in JSX.
Imagine you had an array of book objects in your props that you want rendered to
the DOM. Each book has multiple attributes you want to display, but you don't
need an element that wraps around these attributes. A fragment can be used here,
and can still take a key attribute:

```jsx
const Bookshelf = (props) => {
  return (
    <section>
      {props.books.map((book) => (
        <React.Fragment key={book.id}>
          <h1>{book.title}</h1>
          <h2>{book.author}</h2>
        </React.Fragment>
      ))}
    </section>
  );
};
```

> According to the [React docs](https://reactjs.org/blog/2017/11/28/react-v16.2.0-fragment-support.html#support-for-fragment-syntax):
> As of 16.2.0, support for fragment shorthand syntax `<><p>Hello</p></>` will vary depending on the tools you use to build your app. A stable release may take a bit longer as we await adoption by upstream projects. Support for JSX fragments is available in Babel v7.0.0-beta.31 and above! So for compatibility purposes, it may be better to fully write out `<React.Fragment><p>Hello</p></React.Fragment>` for the time being.

## Conclusion

Fragments are a small addition to React overall, but when used properly, can
reduce a lot of unnecessary DOM bloat. They allow us a bit more flexibility in
how we write our components, eliminating the need for wrapper elements.
