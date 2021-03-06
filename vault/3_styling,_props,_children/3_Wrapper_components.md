# 2. Wrapper components
Created Friday 21 January 2022

#### Why
Suppose we have a component that has a variable component inside it, so that we can render components inside this 'wrapper' component.

This saves us a lot of duplication, like so.
1. Approach A: Don't use wrapper components. For the whole thing, we'd have to copy all the outer-code for the component and then place the target component inside. This has to be done for each instance of "wrapper + core". This references N + 1 files.
2. Approach B: We create a component with a variable 'hole' in it. We then just specify the whole to be the target container.  This references just 2 files.

This is why React allows us to create wrapper components, to enhance DRY (don't repeat yourself).

#### How
- Everything inside a component is actually stored in props, even children and style classes.
- Style classes are not applicable directly on components, unless a change is made inside to include styles inside (by having a concatenated string of classes). Example: `<XYZComponent className="color-2-4" />` is not valid code. But it can be made so.

#### What
Three useful wrapper properties are **named** this:
1. Children - `children`, this is an object.
2. CSS Classes - `className`, it's a string actually. Wrappers may just styles.
3. Styles - `style`, an object.
This makes making wrapper components very easy.

Code for wrapper component. There's no change in the core component.
```JSX
function WrapperComponent(props) {
	return (<div>
				...
				{props.children /*Core component placed here*/}
				<div>
				...
				</div>
				...
			</div>)
}

function CoreComponent() {
	return ...;
}
```

Rendering core in wrapper
```JSX
function WholeComponent {
	return (<WrapperComponent> <CoreComponent/> </WrapperComponent>);
}
```

As it is a wrapper, we now have a closing tag as opposed to non-wrapper components (which are non-closing tags).

###### What about styles on wrappers/components in general?
Just change the `className` string inside the component, like so.
```JSX
function OuterComponent {
	return (<div className={className}>
			...
			</div>);
}
// <OuterComponent className="top red solid"/> would work now


//// OR if own style classes

function OuterComponent2 {
	return (<div className={'color1 ' + className}>
			...
			</div>);
}

// <OuterComponent2 className='solid'/> is actually 'color1 solid'
```
This way classes specified to the component are appended to it, as we wanted.

###### What about outer styles?
Like appending, just use the spread operator.
```JSX
function OuterComponent ({style}) {
	return (<div style={style}>
			...
			</div>);
}

// <OuterComponent style={{color: "red"}}/> works now

//// OR if it has styles of its own

function OuterComponent2 ({style}) {
	return (<div style={{color: "red", ...style}}>
			...
			</div>);
}

// <OuterComponent2 style={{fontStyle:"italic"}} /> is actually 
// style={{color: "red", fontStyle:"italic"}}
```

Example: Real use of wrapper component, [see](https://github.com/exemplar-codes/expense-tracker-react/commit/b5802c373d094c5b993ad06d8ba2f87fdb30e9e8).