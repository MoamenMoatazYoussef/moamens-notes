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

## Testing React
React relies on: "Components produce HTML based on Props and State".
This makes frontend easier to test in React than normal.

There are several JS testing frameworks, like:
- Jest: easy to set up, most popular for React.
- Mocha: highly configurable.
- Jasmine: similar to Mocha.
- Tape: the leanest and simplest of all of them.
- AVA.

We'll use Jest.

Helper libraries:
- React Test Utils: specifically to test react components, a very verbose API, it offers:
	- shallowRender: just rendering a single component without children, no DOM required, fast & simple.
	- renderIntoDocument: renders component and children, DOM required.
		- Libraries like JSDOM helps us use DOM without a browser.
		- Simulating clicks, keypresses, etc.
	Check its documentation.

- Enzyme: using react test utils & JSDom behind the scenes, it's basically the jQuery of React Test Utils.
	- Also, provides jQuery-style selectors using a library called *Cheerio*.
- React Testing Library: alternative to Enzyme, simpler than Enzyme.

We'll explore Enzyme and React Testing Library.

### Jest
Jest have a feature *Snapshot Testing*, this can help with regression testing.

#### Configuring Jest
- Add a script:
``` json
"test": "jest"
```
- Add a section to the main JSON object, "jest".
``` json
"jest": {
	// here, you put configurations, check the docs for that
},
```


#### Testing
Let's start a simple test:
- Create src/index.test.js.
``` js
it('should pass', () => {
	expect(true).toEqual(true);
});
```
- Run the app ```npm test``` or ```npm run test```, these commands both run any file ending with .test.js or .spec.js.
- It should succeed.
- Change one of the true to false, re-run the test.
- It should fail.

We don't need to run the script for every change we do, so change the test script to
``` sh
"test": "jest --watch"
```

#### Snapshot Testing
This records a component's output, and tests against it.
Useful for regression testing.
- Create CourseForm.Snapshots.test.js:
	- Import React, the component to test, *renderer*, and the mock data you need.
``` js
import React from "react";
import CourseForm from "./CourseForm";
import renderer from "react-test-renderer";
import { courses, authors } from "path/to/mockData";

// now, let's test that the label on the Save Button in the form 
// is properly set when we set the save prop to true.

// all snapshot tests start with the it keyword.
// takes two args:
// 1. a description for the test, just a string.
// 2. a function that contains the test case.
it("sets submit button to 'Saving...' when saving is true", () => {
	
	// we'll use renderer to simulate rendering of the CourseForm 
	const tree = renderer.create( 
		<CourseForm 
			course={courses[0]}
			authors={authors}
			onSave={jest.fn()}
			onChange={jest.fn()}
			saving
		/> 
	);
	// when passing the props, we'll use jest.fn()
	// this is an empty mock function so that we don't have to declare
	// our own for the test
	
	// we'll use expect() function to assert the expected behavior
	// we use it, pass a variable to it, then chain other functions to
	// test specific things
	
	// we'll use toMatchSnapshot()
	expect(tree).toMatchSnapshot();
});
```
- Run the test script.
- The test should pass, and it should say that it has written a new snapshot, this will later be used for testing.
- Snapshots are saved in a directory __snapshots___.
- Inside it, you'll find HTML of the output of the component.

Let's create another test, to test if the button label stays the same if we set Saving to false:
``` js
it("doesn't change when saving is false", () => {
	const tree = renderer.create(
		<CourseForm 
			course={courses[0]}
			authors={authors}
			onSave={jest.fn()}
			onChange={jest.fn()}
			saving={false}
		/> 
	);
	expect(tree).toMatchSnapshot();
});
```
- Run the test, it should also pass.

If you change the component later, the tests will fail, but press U after running the tests, this makes Jest update the snapshots based on your new changes, now the test should pass.

### Enzyme
#### Configuration
- Add a new file tools/testSetup.js
	- Import an Adapter for the version of react you're using
``` js
import { configure } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

configure({ adapter: new Adapter() });
```
- Add this to the package.json in tje Jest section:
	- Jest will run any items we declare under setupFiles array, so add the new testSetup.js file to that.
``` json
"jest": {
	"setupFiles" : [
		"./tools/testSetup.js"
	]
}
```

#### Testing with Enzyme
- Add CourseForm.Enzyme.test.js
- Import React, the component to be tested, the functions from enzyme to test stuff, we'll use "shallow" which does shallow rendering (another option is "mount" which does deep rendering).
- Make a function that's a *factory function*, this calls your react component with some default values, to avoid repeating that in each test.
``` js
function renderCourseForm(args) {
	const defaultProps = {
		authors: [],
		course: {},
		saving: false,
		errors: {},
		onSave: () => {},
		onChange: () => {}
	};
	
	const props = { ...defaultProps, ...args };
	return shallow(<CourseForm {...props} />);
}
```
- Now let's add a test that tests if the CourseForm component renders a form and a header.
	- Declare a variable and assign it to the return of the factory function.
	- Use the .find() function from *enzyme* to get specific element from the component using CSS selectors (Like jQuery), use it to get "form" and "h2" since these are the ones we want to test.
	- we need to test that the wrapper contains only one element of Form.
	- the same for h2.
``` js
it('renders form and header', () => {
	const wrapper = renderCourseForm();
	expect(wrapper.find('form').length).toBe(1);
	expect(wrapper.find('h2').length).toBe(1);
});
```
- Run the test, it should pass.

Let's add another test, remember the snapshot test on the save button's label? The same test, in another way:
``` js
it('labels save button as "Saving..." if saving is true', () => {
	const wrapper = renderCourseForm({ saving: true });
	expect(wrapper.find("button").text()).toBe("Saving...");
});
```
- Run the test, it should pass.

When using Shallow rendering, use component "names" in find().
When using mount rendering, use HTML component's names, like **a** for refs and links, in find().
And import and use MemoryRouter for mount rendering.

### React Testing Library
This relies on writing tests based on what the user sees, so it's more realistic.

#### Testing
We'll test that Add Course header is rendered successfully.
- create CourseForm.ReactTestingLibrary.test.js:
``` js
import React from "react";
import { cleanup, render } from "react-testing-library";
import CourseForm from "";

// we'll use cleanup to clean after each one of the tests
afterEach(cleanup);

// here, add the same factory we used, but instead of shallow()
// use render()

it("should render Add Course header", () => {

	// the render() function returns an object with some methods inside
	// it that are used for testing different stuff,
	// we'll get the getByText function because we're looking for a
	// text header
	const { getByText } = renderCourseForm();
	
	// then we'll use that function to run the test
	// note that we don't need to use expect(), the methods from render()
	// has internal assertion
	getByText("Add Course");
});

// another two tests, testing the Save button's label as usual
it("labels as Save when not saving", () => {
	const { getByText } = renderCourseForm({ saving: false });
	getByText("Save");
});

it("labels as Saving... when saving", () => {
	const { getByText } = renderCourseForm({ saving: true });
	getByText("Saving...");
});
```
- Run the tests, they should pass.

React Testing Library's main concern is testing based on what the user sees, so there is no shallow rendering, it's all Mount rendering.

Check the docs for Jest, Enzyme, and RTL for a large collection of testing functions and API.

## Testing Redux
When testing components, we usually test two things:
- Markup: the component should produce a certain output for a certain set of props. (Presentation components mainly)
- Behavior: given a user event, we get some expected behavior.

### Testing Connected Components
The problem is that they're wrapped in connect, so they're not exported, they're wrapped in connect THEN exported.
To test them, we need to do one of these:
- Wrap the component with <Provider> within our test, to be able to pass the store and stuff.
- Export the un-connected component (useful when testing local state and behavior)

Let's try it:
- Create ManageCoursePage.test.js.
- Get the imports: React, mount from enzyme, data from mockData, the component.
- Make a factory function to render your component, like before. 
	- Pass all the props the component needs, including the props injected from redux. 
	- Use mount rendering here. (We need to test the component's interaction with its children and others)
- Write a test that ensures that the form displays a validation error if no course title was passed.
``` js
it("displays validation error", () => {
	const wrapper = render(); //this is the factory function
	wrapper.find("form").simulate("submit"); //a function to simulate user events
	const error = wrapper.find(".alert").first();
	expect(error.text()).toBe("Title is required");
});
```
- Run the test.
- An error will happen, because we need to wrap the component in Provider, or export the normal component.
- Go to the factory function, and wrap it in <Provider/>
	- Or go to the ManageCoursePage.js and just export the component normally, don't remove the connect() export.
- In your test, import the normal plain component.
- Re-run the test.
- It should pass :)

#### Testing Action Creators
ActionCreators return an object, we just need to assert that it returns the expected object.
- Create courseActions.test.js
``` js
import * as courseActions from './courseActions';
import * types from './actionTypes';
import { courses } from '../../../tools/mockData';

describe("createCourseSuccess", () => {
	it("should create CREATE_COURSE_SUCCESS action", () => {
		const course = courses[0];
		const expectedAction = {
			type: types.CREATE_COURSE_SUCCESS,
			course
		};
		
		const action = courseActions.createCourseSuccess(course);
		expect(action).toEqual(expectedAction);
	});
	
	// do the same for the rest of the actions
})
```

#### Testing Thunks
Thunks do two things:
- Dispatch some actions.
- Deal with APIs.

To test them, we need to mock some things:
- Store: using redux-mock-store
- API calls: using fetch-mock

Let's do that:
- In courseACtions.text.js:
``` js
// previous imports here
import thunk from "redux-thunk";
import fetchMock from "fetch-mock";
import configureMockStore from "redux-mock-store";

const middleware = [thunk];
const mockStore = configureMockStore(middleware);

describe("async actions", () => {
	afterEach(() => {
		fetchMock.restore(); 
		//this is to restart the store after each test here
	});
	
	it("should create BEGIN_API_CALL and LOAD_COURSES_SUCCESS", () => {
		//we tell it to capture any fetch calls nad return the body passed
		// as a 2nd argument 
		fetchMock.mock("*", { 
			body: courses,
			headers: {
				"content-type": "application/json"
			}
		});
		
		const expectedActions = [
			// build your actions here
		]
		
		// create a mock redux store and initialize it
		const store = mockStore({ courses: [] });
		
		return store.dispatch(courseActions.loadCourses()).then(() => {
			//write the expect line here
		});
	});
});
```

#### Testing Reducers
Reducers are pure functions, they are so easy to test :)
No mocking or simulating anything, data comes in as an action, so we just call the reducer with a state and an action, and asset the outputs for certain actions.

Let's try it:
- create courseReducer.test.js
- import the reducer and courseActions
``` js
it("should add a course when it's passed CREATE_COURSE_SUCCESS", () => {
	const initialState = [
		{
			title: 'A'
		},
		{
			title: 'B'
		}
	];
	
	const newCourse = {
		title: "C"
	};
	
	const action = actions.createCourseSuccess(newCourse);
	
	const newState = courseReducer(initialState, action);
	
	expect(newState.length).toEqual(3);
	expect(newState.length[0].title).toEqual('A');
	expect(newState.length[1].title).toEqual('B');
	expect(newState.length[2].title).toEqual('C');
});
```
- Create another similar test for UPDATE_COURSE_SUCCESS.
- Run both tests.

Nice :)

#### Testing The Store
Testing the store means testing that actions, reducers, and the store are all interacting property together.

So, this is kind of an integration test.
- Create redux/store.test.js.
- Import createStore, rootReducer, initialState, and courseActions.
- Write a test to test that the store creates courses.
	- Create a new store using createStore, pass the rootReducer and initialState.
	- Make a dummy course.
	- Make a dummy action using createCourseSuccess, pass the dummy course.
	- Dispatch that action using the store.dispatch()
	- Get the new course like this: ```store.getState().course[0];```.
	- Expect the created course to equal the dummy course.


## Production Builds and Deployment




