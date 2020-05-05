## REDUX

Main goal of Redux is to make the state management of an application more predictable.

![](d.png)
![](e.png)

In this example, the app appears exactly the same to the end user, however, it's functioning quite differently under the hood. All of the data is stored outside of the UI code and is just referenced from the UI code.

With a change like this, if the data needs to be modified at all, then all of the data is located in one place and needs to be only changed once. Then the areas of the app that are referencing pieces of data, will be updated since the source they're pulling from has changed.

State Tree
One of the key points of Redux is that all of the data is stored in a single object called the state tree. But what does a state tree actually look like? Here's an example:

```js
{
  recipes: [
    { … },
    { … },
    { … }
  ],
  ingredients: [
    { … },
    { … },
    { … },
    { … },
    { … },
    { … }
  ],
  products: [
    { … },
    { … },
    { … },
    { … }
  ]
}
```

See how all of the data for this imaginary cooking site is stored in a single object? So all of the state (or "application data") for this site is stored in one, single location. This is what we mean when we say "state tree"...it's just all of the data stored in a single object.

- . We saw that in traditional apps, the data is mixed in with the UI and markup. This can lead to hard-to-find bugs where updating the state in one location doesn't update it in every location.

We learned that the main goal that Redux is trying to offer is predictable state management. The way that Redux tries to accomplish this is through having a single state tree. This state tree is an object that stores the entire state for an application. Now that all state is stored in one location, we discovered three ways to interact with it:

getting the state - getStore()
listening for changes to the state - subscribe(listener)
updating the state - Dispatcher(dispatch(action)), Actions, Action Creator and Reducer

Then we combine the three items above and the state tree object itself into one unit which we called the store.
![](f.png)

```js
function createStore () {
  // The store should have four parts
  // 1. The state
  // 2. Get the state.
  // 3. Listen to changes on the state.
  // 4. Update the state

  let state
  let listeners = []
  const getState = () => state

  const subscribe = (listener) => {
    listeners.push(listener)
    <!-- if function is called again it will unsubscribe to that listener -->
    return () => {
      listeners = listeners.filter((l) => l !== listener)
    }
  }

  return {
    getState,
    subscribe
  }
}

cosnt store = new createStore()
store.subscribe(()=>{
  console.log(":Listener 1" , store.getState())
})

store.subscribe(()=>{
  console.log(":Listener 2" , store.getState())
})


```

- Updating the store state
- we don't want everyone to update the state hence we have some guards hired out know as Action event
  ![](g.png)
  When an event takes place in a Redux application, we use a plain JavaScript object to keep track of what the specific event was. This object is called an Action.

Let's take another look at an Action:

```js
{
  type: "ADD_PRODUCT_TO_CART";
}
```

As you can see, an Action is clearly just a plain JavaScript object. What makes this plain JavaScript object special in Redux, is that every Action must have a type property. The purpose of the type property is to let our app (Redux) know exactly what event just took place. This Action tells us that a product was added to the cart. That's incredibly descriptive and quite helpful, isn't it?

Now, since an Action is just a regular object, we can include extra data about the event that took place:

```js
{
  type: "ADD_PRODUCT_TO_CART",
  productId: 17
}
```

In this Action, we're including the productId field. Now we know exactly which product was added to the store!
Action Creators are functions that create/return action objects. For example:

```js
const addItem = (item) => ({
  type: ADD_ITEM,
  item,
});
```

And we've got our second rule!

The function that returns the new state needs to be a pure function.

So far, our rules are:

Only an event can change the state of the store.
The function that returns the new state needs to be a pure function.
![](h.png)

What are Pure Functions?
Pure functions are integral to how state in Redux applications is updated. By definition, pure functions:

Return the same result if the same arguments are passed in
Depend solely on the arguments passed into them
Do not produce side effects, such as API requests and I/O operations

- These pure functions that takein the state and action are known as Reducers.

```js
// Library Code
function createStore(reducer) {
  // The store should have four parts
  // 1. The state
  // 2. Get the state.
  // 3. Listen to changes on the state.
  // 4. Update the state

  let state;
  let listeners = [];

  const getState = () => state;

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter((l) => l !== listener);
    };
  };

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach((listener) => listener());
  };

  return {
    getState,
    subscribe,
    dispatch,
  };
}

// App Code
function todos(state = [], action) {
  if (action.type === "ADD_TODO") {
    return state.concat([action.todo]);
  }

  return state;
}
```

## We've finally finished creating the createStore function! Using the image above as a guide, let's break down what we've accomplished:

we created a function called createStore() that returns a store object
createStore() must be passed a "reducer" function when invoked
the store object has three methods on it:
.getState() - used to get the current state from the store
.subscribe() - used to provide a listener function the store will call when the state changes
.dispatch() - used to make changes to the store's state
the store object's methods have access to the state of the store via closure

```js
store.subscribe(() => {
  console.log("The new state is: ", store.getState());
});

store.dispatch({
  type: "ADD_TODO",
  todo: {
    id: 0,
    name: "Learn Redux",
    complete: false,
  },
});
```

```js
function todos(state = [], action) {
  switch (action.type) {
    case "ADD_TODO":
      return state.concat([action.todo]);
    case "REMOVE_TODO":
      return state.filter((todo) => todo.id !== action.id);
    case "TOGGLE_TODO":
      return state.map((todo) =>
        todo.id !== action.id
          ? todo
          : Object.assign({}, todo, { complete: !todo.complete })
      );
    default:
      return state;
  }
}

function goals(state = [], action) {
  switch (action.type) {
    case "ADD_GOAL":
      return state.concat([action.goal]);
    case "REMOVE_GOAL":
      return state.filter((goal) => goal.id !== action.id);
    default:
      return state;
  }
}
```

kind of pointed it out at the end of the previous screencast, but we now have two reducer functions:

todos
goals
However, the createStore() function we built can only handle a single reducer function:

// createStore takes one reducer function as an argument
const store = createStore(todos);
We can't call createStore() passing it two reducer functions:

// this will not work
const store = createStore(todos, goals);

<!--  so we combine these reducers into a large reducer know as root Reducer -->

```js
function rootReducer(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    goals: goals(state.goals, action),
  };
}
```

- Reducers must be pure
- Though each reducer handles a different slice of state, we must combine reducers into a single reducer to pass to the store
- createStore() takes only one reducer argument
- Reducers are typically named after the slices of state they manage

- Prefer constants rather than strings as the values of type properties.
  Both work -- but when using constants, the console will throw an error rather than fail silently should there be any misspellings (e.g. LOAD_PROFIEL vs. LOAD_PROFILE)

## Middleware

You’ve learned how Redux makes state management more predictable: in order to change the store’s state, an action describing that change must be dispatched to the reducer. In turn, the reducer produces the new state. This new state replaces the previous state in the store. So the next time store.getState() is called, the new, most up-to-date state is returned.

Between the dispatching of an action and the reducer running, we can introduce code called middleware to intercept the action before the reducer is invoked. The Redux docs describe middleware as:

> > …a third-party extension point between dispatching an action, and the moment it reaches the reducer.

What's great about middleware is that once it receives the action, it can carry out a number of operations, including:

- producing a side effect (e.g., logging information about the store)
- processing the action itself (e.g., making an asynchronous HTTP request)
- redirecting the action (e.g., to another piece of middleware)
- dispatching supplementary actions
- or even some combination of the above! Middleware can do any of these before passing the action along to the reducer.

> > Middleware is the suggested way to extend Redux with custom functionality.

## Redux in Apps

![](./images/s.png)

## Async Redux and Reduc Thunk

- If a web application requires interaction with a server, applying middleware such as thunk helps solve the issue of asynchronous data flow. Thunk middleware allows us to write action creators that return functions rather than objects.

- By calling our API in an action creator, we make the action creator responsible for fetching the data it needs to create the action. Since we move the data-fetching code to action creators, we build a cleaner separation between our UI logic and our data-fetching logic. As a result, thunks can then be used to delay an action dispatch, or to dispatch only if a certain condition is met (e.g., a request is resolved).

## Optimistic Updates

- When dealing with asynchronous requests, there will always be some delay involved. If not taken into consideration, this could cause some weird UI issues. For example, let’s say when a user wants to delete a todo item, that whole process from when the user clicks“delete” to when that item is removed from the database takes two seconds. If you designed the UI to wait for the confirmation from the server to remove the item from the list on the client, your user would click “delete” and then would have to wait for two seconds to see that update in the UI. That’s not the best experience.

- Instead what you can do is a technique called optimistic updates. Instead of waiting for confirmation from the server, just instantly remove the user from the UI when the user clicks “delete”, then, if the server responds back with an error that the user wasn’t actually deleted, you can add the information back in. This way your user gets that instant feedback from the UI, but, under the hood, the request is still asynchronous.
