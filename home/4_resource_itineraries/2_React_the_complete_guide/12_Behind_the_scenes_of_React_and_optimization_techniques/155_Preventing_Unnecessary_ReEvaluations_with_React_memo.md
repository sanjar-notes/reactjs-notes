# 155. Preventing Unnecessary Re-Evaluations with React.memo()
Created Sunday 10 July 2022

### Why
- In the last page, we acknowledged that in React, when a component re-evaluates, so do all of it's constituents, even if their props haven't change (which includes not having any). This *is* inefficient.

### How
- Solving the problem
	- The solution is simple, just compare the old and new props and run the component function only if there's a change. If there's no change, return the stored copy of the render output, i.e. don't compute it again. React provides this functionality out of the box.
	- This optimization is a trade-off though - prop diffing vs function re-evaluation. 
		- And it depends (on number/complexity of props, number of child components) which is costly, so use `React.memo` with care.
		- If it's known that props will change frequently, it's better to avoid using `React.memo`.
		- It's better to use `React.memo` to avoid re-evaluations of whole UI tree sections, of course knowing that props don't change very frequently.
	- Why isn't this 'prop-diffing' the default - because of the overhead of prop-diffing.
- Syntax
	Just wrap the component function, for which you want to do prop diffing of *received props*, with `React.memo()`. So, a normal component like this:
	```jsx
	const MyComponent = (props) => { return <> ...</>; };
	
	export default MyComponent;
	```
	becomes
	```jsx
	import React from 'react'; // cannot be skipped if using memo
	const MyComponent = (props) => { return <> ...</>; };
	
	export default React.memo(MyComponent);
	
	
	// OR equivalently
	const MyComponent = React.memo((props) => { return <> ...</>; });
	
	export default MyComponent;
	```
	`React.memo` can also be used on components that don't receive props.
- How does the hook work? My guess:
	- React keeps a copy of two things internally:
		- Props received in the latest evaluation.
		- Return value of the latest evaluation.
	- If props have changed, it re-evaluates the function and stores both props and return value. And returns the return value to the parent.
	- If props haven't changed, it just returns the stored return value of the latest evaluation to the parent.
	- Note: the return value is stored "kind of" in the component itself (which was wrapped in `React.memo`), and not it's parent. This means that the parent is oblivious to the fact that it received a stored value instead of a newly computed one. FIXME(I'm not sure if the both props and return value are stored in the parent or not. Also need to find out if the parent knows if one of it's children value was a stored value, not a computed one, maybe the parent is aware, idk). 