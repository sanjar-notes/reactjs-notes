# 1. Components
Created Tuesday 02 November 2021

FIXME - all here

#### Why
State needs to be passed to the elements being rendered. This can be done in components.

#### How
By passing a ``props`` argument to the component, this is optional though.

#### What

* These are different from elements.
* They are either functions or classes (with a render method) that have a ``props`` argument that contains the state.


Note: ``props`` are read only, by convention. Let the components be pure, w.r.t their ``props``.

