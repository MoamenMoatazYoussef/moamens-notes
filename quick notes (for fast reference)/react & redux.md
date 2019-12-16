# React with Redux quick notes

## Why redux?
It has these special features:
- It keeps all state in One centralized object graph.
- Avoids data duplication.
- Reduces a lot of boilerplate.
- Its arch is friendly to servers.
- Immutable store.
- Enables hot reloading without losing client-side state.
- Small.

## Environment Build
There are hundreds of environment builds we can use like:
- create-react-app
- next.js
- gatsby
etc.

We'll create our own react app environment to understand and be able to customize our dev environment.
We will build an enviroment that:
- compiles jsx
- transpiles js
- lints our js
- generates index.html
- auto reloads on save

We will need to install:
- Node.js 8 or higher.
- VS Code (or any code editor).
- Prettier, a code formatter. (We'll install it as a plugin in VS code)
- Babel.
- Webpack.

### Babel
Transpiler for next-gen JS and JSX.
- Install babel, babel-loader.
- Add this to package.json:
``` json
  "babel": {
    "presets": [
      "babel-preset-react-app"
    ]
  }
```

### npm scripts to start the app
Add these to package.json.scripts
``` json
  "scripts": {
    "start": "webpack-dev-server --config webpack.config.dev.js --port 3000"
  },
```

Now npm start it.

### EADDRINUSE error
E address in use error.
If we tried to run another instance of the app, we'll get that error.

### ESLint for linting
ESLint: alerts us when we make mistakes.
Configuring ESLint:
``` json
"eslintConfig": {
    "extends": [ //Enables settings for react, importing errors and warnings
      "eslint:recommended",
      "plugin:react/recommended",
      "plugin:import/errors",
      "plugin:import/warnings"
    ],
    "parser": "babel-eslint", //since we're using babel
    "parserOptions": {  // which version of ecmascript and stuff
      "ecmaVersion": 2018,
      "sourceType": "module",
      "ecmaFeatures": {
        "jsx": true
      }
    },
    "env": { // environments that we'll work in, each as an enviroment valuable
      "browser": true,
      "node": true,
      "es6": true,
      "jest": true
    },
    "rules": {  // overriding some rules e.g. "debugger" and "console.log" usage, we'll disable the warnings
      "no-debugger": "off",
      "no-console": "off",
      "no-unused-vars": "warn",
      "react/prop-types": "warn"
    },
    "settings": { //giving the react version or letting it detect the version
      "react": {
        "version": "detect"
      }
    },
    "root": true
  }
```

- Add this to webpack.config.dev.js
module.exports.module.rules[0].use["eslint-loader"]

- Finally, install eslint & eslint-loader.

### Summary of enviroment
Our environment
- Babel: compiles jsx
- Babel: transpiles js
- ESLint: lints our js
- Webpack: generates index.html
- webpack-dev-server: auto reloads on save
- webpack-dev-server: servs the files


## React component approaches
There are four ways to create React components
- createClass
- ES classes
- Functions
- Arro functions

1. createClass: the classic way
``` js
var theComponent = React.createClass({
    render: function() {
        return <div>Hello World</div>
    }
})
```

2. ES classes: the way most people like
``` js
class theComponent extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return <div>Hello World</div>
    }
}
```

3. Functional components: the simpler way
``` js
function theComponent(props) {
    return <div>Hello World</div>
}
```

4. Arrow functions: the simplest way
``` js
const theComponent = (props) => <div>Hello World</div>
```

### When to use each?
Functional components:
- are easier to understand
- don't need binding of 'this'.
- transpile smaller than class components i.e. less code, better performance.
- High SNR.
- Easier to test.
- As of react 16, there's no instance created to wrap them.
- Classes are likely to be removed in the future thanks to React Hooks.

So:
1. Prefer functional components over class components in general.
2. If you really need local state, refs, lifecycle methods, and your react is lower than 16, use class components. Else use functional components with hooks.
3. If you need componentDidError or getSnapshotBeforeUpdate, and your react is 16 or higher, use class components. Else use functional components with hooks.

### Container vs Presentation components
Container components: 
    - contain the logic, very little markup. It's like a backend for the frontend. (Components don't have to emit DOM)
    - usually stateful.
    - interact with redux.
    - in redux, created using redux connect() function
Presentation components: 
    - only contain the markup.
    - receive data and actions from container components.
    - don't deal with redux at all.
    - usually not stateful.

Most of your components in your redux app should be presentation.
If you find yourself passing props down a lot of components from the main container component to a far presentational component, it's a good idea to introduce a new container component in the middle layers that connects to redux and actually handles logic.

## When to use redux
- Do you have complex data flows between the components?
- Components that are interacting but are not children of each other e.g. peers, etc. ?
- Components that manipulate the same data?
- Many actions e.g. lots of write, read, delete for complex data structures.
- Using the same data in many places, usually this is the main need for redux.

Steps to follow:
1. Begin with normal local state.
2. Lift state to common parents.
3. If lifting states is annoying or not scalable, go for context or redux.

>>> Flux VS Redux video

### actions
The only thing you don't pass to actions are things that won't serialize to JSON e.g. functions, Promises.

### Immutability
Immutability means: instead of changing the original object, return a new object represents the current state.
In JS, Number, String, Boolean, Undefined, Null, all of these are already immutable.
In JS, Objects, Arrays, and Functions are mutable.

#### Easy copying of objects in JS for immutability
- Object.assign: creates a new object using an existing object(s) as a template (if you pass many objects, it will merge them).
- the spread operator
- array methods like map, filter, reduce.

AVOID push, pull, and reverse array methods, they MUTATE the array.
Use map, filter, reduce, concat, spread.

``` js
const state = {
  name: "Moamen",
  role: "Developer"
}

const newStateExample1 = Object.assign({}, state, { role: "Fullstack Developer" }); 
// returns a new object that's a merge between state and the other object.
// so the return is a copy of state object BUT with role overridden with the new value.
// WARNING: the first object MUST BE an empty object, if you leave it out, you will mutate the state!

const newStateExample2 = {...state, role: "Fullstack Developer"};
// the spread operator creates a shallow copy of "state", then merged with object values on the right.

// NOTE: both Object.assign and ... creates a SHALLOW copy
// nested objects need to be explicitly copied 
// else, you will reference the original copy, which makes it MUTABLE
// but that's actually preferred if you won't change 
// the internal values of the nested object
// because deep cloning costs a lot of performance and causes unnecessary renders
// since react may thing everything has changed.

//
```
Or just use a library like immutable.js or immer. You write your normal code, and they handle the immutability.

#### Why make state immutable anyway?
- Clarity: we don't have to search for where the state was updated, we know it was in a REDUCER.
- Performance: To know if a state has changed, normally we would check & compare every value, but if it's immutable and a new copy, we can just compare references, which is extremely efficient. Also, redux doesn't notify react to render a component if nothing is changed, so much less unnecessary renders.  
- Debugging: you can use redux dev tools to see each copy of the state and monitor what happened to the state.

### Don't do these in reducers
- mutate args
- perform side effects e.g. api calls, routing, etc.
- call other non-pure functions e.g. Date.now(), Math.random(), etc.

When dispatching actions, ALL reducers are called, that's why any reducer MUST return the untouched state as default.

## Main components of Redux we'll use with container components
- <Provider /> :This element attached the whole app to Redux.
  It uses React Context API for this.
  ``` js
  () => {
    return(
        <Provider store={this.props.store}
          <App/>
        </Provider>
      )
    }
  ```
  We use that once.
- connect(mapStateToProps, mapDispatchToProps): it wraps a component so that it's connected to the redux store. Both params are optional.
  - mapStateToProps: what state arguments to pass to that component as props.
    - The component will subscribe to the redux store updates when you pass this function. 
    - Anytime the state updates, this function will be called.
    - And anytime the props change, the component re-renders.
  - mapDispatchToProps: what actions to pass to that component as props.


How to pass actions using mapDispatchToProps?
1. ignore the function, so you expose NO actions.
  - You can call dispatch manually and pass action creators.
  - But that's cumbersome and you'll need to reference redux specifically.
2. Wrap actions manually.
  ``` js
  const mapDispatchToProps = (dispatch) {
    return {
      loadCourses: () => { dispatch(loadCourses()); },
      createCourse: () => { dispatch(createCourse()); },
      updateCourse: () => { dispatch(updateCourse()); },
    }
  }
  ```
  This makes the actions accessible using this.props, so you can do ```this.props.loadCourses()```.
  It's good and works, but kinda redundant.
3. bindActionCreators(): a function that we call and it wraps actions passed to it in a dispatch call for you i.e. it will do the 'wrap manually' work.
  ``` js
  const mapDispatchToProps = (dispatch) {
    return {
      actions: bindActionCreators(actions, dispatch)
    };
  }
  ```
  Now, you can use this ```this.props.actions.loadCourses()```. (Notice the difference)
4. mapDispatchToProps as object:
  ``` js
  const mapDispatchToProps = {
      loadCourses,
  }
  ```
  Binding is also done, and it's much easier.

## async activities in react redux

### Mock APIs
An api that you create to develop upon, without waiting for backend.
We'll use JSON server for that.
Then add this script to package.json:
``` json
"prestart:api" : "node tools/createMockDb.js"
```
Note: any npm script prefixed with **pre** will run BEFORE the npm start.

## Middleware in redux
It runs between action dispatching, and the moment it reaches the reducer.
It can:
- handle async API calls.
- logging
- reporting crashes
- routing
- etc.
Basically cross-cutting concerns are handled by middleware.
We can write our own middleware, or use middlewares provided by the redux community.

Actions are sync objects, so how do we handle async calls?
We can use libraries like:
- redux-thunk
- redux-promise
- redux-saga
and others.

We'll use thunk.

### Redux Thunk
"Thunk": in CS, it means a function that wraps an expression in order to delay its evaluation.
Redux thunks, from an action creator, it returns a function instead of an object.

btw, redux thunk is 14 lines of code.

How to use thunks:
- in configureStore.js, import Thunk from "redux-thunk".
- add "Thunk" to applyMiddleware.

Then,
- open courseActions.js, import courseApi.
- write a function (and export it) that returns "a function that accepts dispatch as argument".
- inside it, return the async call itself.
``` js
export function loadCourses() {
    return function (dispatch) {
        return courseApi.getCourses()
            .then(courses => {
                dispatch(loadCoursesSuccess(courses))
            })
            .catch(error => {
                throw error;
            })
    }
}
```
- then add this new action: (for our app, not to create a thunk, that previous function is a thunk)
``` js
export const loadCoursesSuccess = (courses) => ({
    type: actions.LOAD_COURSES_SUCCESS,
    courses
});
```
- add the new reducer part:
``` js
        case actions.LOAD_COURSES_SUCCESS:
            return actions.courses;
```
That's it, because whatever will be returned from our API will replace the state's corresponding values, we don't have to merge the state here. (It won't harm, but we don't have to)

Then, we'll call that API when the courses page is opened.
- in coursesPage, declare the componentDidMount functions, then call the action loadCourses() i.e. call the THUNK, not the direct ACTION.


## When to use react state
Don't use Redux state on ALL state.
Only use react state when the data inside that state is used by one or few components.
Redux state for more global data.

