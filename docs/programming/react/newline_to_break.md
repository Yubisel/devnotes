# Newline to break (nl2br)

Source [https://kevinsimper.medium.com/react-newline-to-break-nl2br-a1c240ba746](https://kevinsimper.medium.com/react-newline-to-break-nl2br-a1c240ba746)

Because you know that everything in React is functions, you can't really do this

```js
this.state.text.replace(/(?:\r\n|\r|\n)/g, '<br />')
```

Since that would return a string with DOM nodes inside, that is not allowed either, because has to be only a string.

You then can try do something like this:

```js
{this.props.text.split('\n').map(function(item, key) {
  return (
    <span key={key}>
      {item}
      <br/>
    </span>
  )
})};
```

That is not allowed either because again React is pure functions and two functions can be next to each other.

## tldr. Solution

```js
{this.props.text.split('\n').map(function(item, key) {
  return (
    <span key={key}>
      {item}
      <br/>
    </span>
  )
})};
```

Now we're wrapping each line-break in a span, and that works fine because span’s has display inline. Now we got a working nl2br line-break solution.

And ES6 version

```js
{this.props.text.split('\n').map((item, key) => {
  return <span key={key}>{item}<br/></span>
})};
```

And with React Fragments

```js
{this.props.text.split('\n').map((item, key) => {
  return <Fragment key={key}>{item}<br/></Fragment>
})};
```
