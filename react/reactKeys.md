# Keys and Iterated children 

This is in continuation of `Reconciliation` and `Virtual DOM`

As a part of diffing when applying updates to the DOM, React first applies the aformentioned rules to update the tree and the recurses on the children. This recursion itself presents various scenarios and use cases.

Let us consider an unordered list:

```html
<ul>
  <li>Apple</li>
  <li>Mango</li>
</ul>
```

A child `li` is added to the list like so: 

```html
<ul>
  <li>Apple</li>
  <li>Mango</li>
  <li>Orange</li>
</ul>
```

No when recursing on the children of `ul` React compares both the list of children together and creates a mutation when a diff is detected. In this case it's found that an extra child `li` has been added to the `ul` and it's added. This is quick as it just includes a single mutation (i.e. Adding a child to `ul`)

But now we consider an opposite use case but the child is added to the starting of the list like so: 

```html
<ul>
  <li>Orange</li>
  <li>Apple</li>
  <li>Mango</li>
</ul>
```

React begins to recurse on the children and immediately begins to build a new tree since the first children don't match. It does not realize that the remaining 2 elements have just been moved and don't need to be recreated. This is an efficiency which can be solved using the `key` prop

## Enter the `key` prop

Now consider the old tree with a `key` prop added:

```html
<ul>
  <li key="example-key-1">Apple</li>
  <li key="example-key-2">Mango</li>
</ul>
```

We now add a child `li` with a `key` prop of its own

```html
<ul>
  <li key="example-key-3">Orange</li>
  <li key="example-key-1">Apple</li>
  <li key="example-key-2">Mango</li>
</ul>
```
**React uses the key to match children in the original tree with children in the new tree.**

By supplying the key prop React can reuse the `example-key-1` and `example-key-2` without neeeding to rebuild the whole tree. This can cause a significant performance boost.

Keys can be assigned using the following methods:

1. If the data comes from a backend it is possible it already has an `id` or `_id` property. That can be readily used as is.
2. Hashing the internal content of the child. For example hashing the `innerHTML` of the above `li` elements to get a `key`
3. Just using the item's index when using a `map` to render the elements. This has pitfalls when items are expected to be re-ordered.

Keys should be stable, predictable, and unique
