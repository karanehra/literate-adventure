# Virtual DOM in React

React keeps a "virtual" representation of the DOM tree in memory and synca it with the Real DOM when Lifecycle Events are triggered via a process called `Reconciliation`

Reconciliation is done using `diffing` algorithms to find out exactly what updates have to be applied to the DOM tree.

Tree diffing algorithms have large time complexities but React has an `O(n)` algorithm based on the following assumptions:
1. Two different elements produce two different trees.
2. The developer can hint at which child elements may be stable across different renders with a key prop.

# Diffing Cases

The diffing happens as a recursive top down process starting from the root node

## Root Element Types are Different
Cases where the type of root element changes (ex: from `div` to `span`) then react tears-down and rebuilds the whole tree from scratch. 

Components receive `componentWillUnmount` upon teardown. Upon rebuild `componentWillMount` and `componentDidMount` are sent to components. 

Old state is lost.

## Root Element Types are Same

In this case React compares the attributes keeping the underlying element the same. Attributes that are changed are updated. For object attributes like `style` React also does a deep comparision of the two style attributes and updates only the diff