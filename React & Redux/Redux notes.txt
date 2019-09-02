Redux notes:
============
Tips:
-----
- This note is compiled from my completing the challenges of Redux and React-Redux on freeCodeCamp, 
    it's not an 'official' source, I'm willing to read the book 'Taming the state of react with redux' sometime in the future
    but I wanted to learn redux ASAP because I need it in my current task, that's why I picked freeCodeCamp and not a more detailed
    source, when I finish the book I'll make notes on it, maybe I'll add it here or make a separate note.
- Anything between //// and //// is code in the language who's name is written after the first ////.
- This notes assumes that you are familiar with React.
- Anything after >> is a command in a command prompt, terminal, or bash.
- Any ES6-related syntax will NOT be explained here, but the title will be enclosed by *** <title> ***
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Introduction:
-------------
What is redux?
--------------
Redux is a state management framework, in it there's a single state object responsible for the entire state of your app.
For example if we have a react app with 10 components each with its local state, the entire state of the app would be defined by ONE object.

That one object is called the 'store'.
So, the store is the single source of truth when it comes to application state.

So, any component that wishes to update the state, it will need to do that through the store.

Why redux?
----------
This makes it much easier to track the state of your app.

Now we'll cover how to create a store and put the state into it, how to read that state, and how to update it.

1) Creating a store:
We need something called a 'reducer', we'll cover that later. 
//// Redux
const reducer = (state = 5) => {
    return state;
}

const store = Redux.createStore(reducer);
//// ENDCODE

2) Reading the state:
Okay, now we have a store object with a state, we need to read the state from the store.
//// Redux
let currentState = store.getState();
//// ENDCODE

3) Updating the state:
Here, we don't do something like this.setState, this is called mutating the state, we don't change the state directly like that.
Instead, we 'trigger' state updates by somethings called 'actions'.

If you're in a team working on a project, the team's lead knows the state of the project and based on it assigns tasks.
If there are two tasks that depend on your task, then ONLY when you finish your task, can the team lead assign the two tasks 
to two people.
So, when you finish your task, you go and tell the lead that you finished your task.
Then, the lead assigns the next two tasks to two people.

Team lead = store.
You = component.
Tell the lead that an event happened = dispatch an action.

So, the store lies there waiting for actions to happen, when a component tells it that an action has happened 
(i.e. dispatches an action), the store updates the state accordingly.
Then, any component interested in the state change would read the state and modify its behavior accordingly.
It's like a PubSub but advanced.

A redux action is a JS object that has:
- A type property that has a unique string value.
- (Optional) It may carry data with it.

How to declare an action:
//// Redux
const action1 = {
  type: 'LOGIN'
};
//// ENDCODE

Now we have an action definition, when an event occurs that's related to that action definition, we want to create 
an action object to be sent to the store.

For that, we need a JS function called an 'actionCreator', it's a function that returns an action.

Creating an actionCreator.
//// Redux
function actionCreator() {
    return action;
}
//// ENDCODE

Now we have our action object, we are ready to send it to the store, this is called 'dispatch'.

A global method provided by Redux is used to do that.
store.dispatch(action);

Here's the usage:
//// Redux
//Here's a store
const store = Redux.createStore(
  (state = {login: false}) => state
);

//And here's an action creator
const loginAction = () => {
  return {
    type: 'LOGIN'
  }
};

//and here's us creating an object and sending it to the store:
store.dispatch(loginAction());
//// ENDCODE

Action processing:
------------------
But how will the store respond to that action?
It should respond by changing the state in some way.
If we have a lot of actions, we have a lot of ways to change the state.
So, each way is linked to each action.
A 'way of changing the state' is a function, called a 'reducer' function.

Reducers modify the state according to actions that occur.
They take 'state' and 'action' as arguments, and they return a new 'state'.

This is the ONLY role of the reducer, it never calls API, it never has anything else, it's a pure function that takes 
a state and action, and returns a state.

In redux, the state is read-only, you can't change it.
Reducers CREATE a new state out of the old one, and return it.
But redux doesn't enforce this, it's your responsibility to enforce this as a developer.

Example:
//// Redux
const defaultState = {
  login: false
};

const reducer = (state = defaultState, action) => {
  // change code below this line
  if(action.type === 'LOGIN') {
  return {
    login: true
  }
  } else {
    return state;
  }
};

const store = Redux.createStore(reducer);

const loginAction = () => {
  return {
    type: 'LOGIN'
  }
};
//// ENDCODE

To make a reducer that handles many actions, we use switch statement.
In that case, Always have the default statement, it returns the current state.

Tip:
- it's a convention to make action type values constant THEN assign then to actions.
- Also, use these values inside the reducer's switch statement.

Example:
//// Redux
const defaultState = {
  authenticated: false
};

const authReducer = (state = defaultState, action) => {
  // change code below this line
  switch(action.type) {
    case LOGIN:
      return {
        authenticated: true
      };

    case LOGOUT:
      return {
      authenticated: false
    }
    default:
      return state;
  }
  // change code above this line
};

const store = Redux.createStore(authReducer);

const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';

const loginUser = () => {
  return {
    type: LOGIN
  }
};

const logoutUser = () => {
  return {
    type: LOGOUT
  }
};
//// ENDCODE


Okay, now we covered how to:
- create a store.
- put a state in it.
- create an action definition.
- create an action object.
- dispatch the action to the store.
- define a reducer that changes the state according to the action.

Listeners:
----------
When the state changes, some components might be interested in that change. 
These are called 'listeners'.

listeners are functions that are called whenever an action is dispatched to the store.
They are callback functions.

To make a function 'listen' to the state change in the store, they use the 'store.subscribe(listenerFunction)' method:
//// Redux
const ADD = 'ADD';

const reducer = (state = 0, action) => {
  switch(action.type) {
    case ADD:
      return state + 1;
    default:
      return state;
  }
};

const store = Redux.createStore(reducer);

// global count variable:
let count = 0;

// change code below this line
function addListener() {
  count++;
}

store.subscribe(addListener);
// change code above this line

store.dispatch({type: ADD});
console.log(count);
store.dispatch({type: ADD});
console.log(count);
store.dispatch({type: ADD});
console.log(count);
//// ENDCODE

Combining multiple reducers:
----------------------------
When the app gets bigger, it's tempting to divide the state into smaller pieces.
But, that violates the Redux concept.
So, instead we can divide the reducer into many smaller reducers, each handles a different piece of the state.


How?
- We define many reducers, 
- then compose them into a root reducer, 
- then pass that to redux createStore method.

To do that, we use the function combineReducers().
It accepts an object as argument, in the object we define properties which associate keys to specific reducers.
So, each reducer will modify that part of the state only.

Here, we have two reducers, and we use combineReducers to combine them into a root reducer, then pass it to the store 
when we create it:
//// Redux
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const counterReducer = (state = 0, action) => {
  switch(action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
};

const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';

const authReducer = (state = {authenticated: false}, action) => {
  switch(action.type) {
    case LOGIN:
      return {
        authenticated: true
      }
    case LOGOUT:
      return {
        authenticated: false
      }
    default:
      return state;
  }
};

const rootReducer = Redux.combineReducers({
  count: counterReducer,
  auth: authReducer
});

const store = Redux.createStore(rootReducer);
//// ENDCODE

Sending data to the store via actions:
--------------------------------------
//// Redux
const ADD_NOTE = 'ADD_NOTE';

const notesReducer = (state = 'Initial State', action) => {
  switch(action.type) {
    // change code below this line
    case ADD_NOTE:
      return action.text;
    // change code above this line
    default:
      return state;
  }
};

const addNoteText = (note) => {
  return({
    type: ADD_NOTE,
    text: note
  });
};

const store = Redux.createStore(notesReducer);

console.log(store.getState());
store.dispatch(addNoteText('Hello!'));
console.log(store.getState());
//// ENDCODE

Async processing in redux:
--------------------------
At some point we'll need to handle async stuff, how do we handle that in Redux?
Redux has a middleware called Thunk, designed for this purpose.

To include it, we pass it as argument in the function Redux.applyMiddleware().
Then, that statement is provided as a second argument to createStore().
//// Redux
const store = Redux.createStore(
  asyncDataReducer,
  Redux.applyMiddleware(ReduxThunk.default)
);
//// ENDCODE

To create an async action, instead of creating it using a normal action creator, we make a function that RETURNS an 
action creator function and pass it 'dispatch' as an argument.

Example: 
- an async request is simulated with a setTimeout function here,
- it's common to dispatch actions BEFORE initiating any async behavior so that the store knows that some data is being 
requested (so that it can display a loading icon for example).
- Then, once the data is received, we dispatch another action which carries the data as payload along with info that 
the action is completed.
//// Redux
const handleAsync = () => {
    return function(dispatch) {
        //here we dispatch request actions.

        setTimeout(function() {
            let data = {
                //some data
            }
            //also we can dispatch received data actions here.
        }, 2500);
    }
}
//// ENDCODE

So now, we'll use that actionCreator to dispatch actions by passing them directly and the middleware takes care of the rest.

//// Redux
const REQUESTING_DATA = 'REQUESTING_DATA'
const RECEIVED_DATA = 'RECEIVED_DATA'

const requestingData = () => { return {type: REQUESTING_DATA} }
const receivedData = (data) => { return {type: RECEIVED_DATA, users: data.users} }

const handleAsync = () => {
  return function(dispatch) {
    store.dispatch(requestingData());

    setTimeout(function() {
      let data = {
        users: ['Jeff', 'William', 'Alice']
      }
      
      store.dispatch(receivedData(data));

    }, 2500);
  }
};

const defaultState = {
  fetching: false,
  users: []
};

const asyncDataReducer = (state = defaultState, action) => {
  switch(action.type) {
    case REQUESTING_DATA:
      return {
        fetching: true,
        users: []
      }
    case RECEIVED_DATA:
      return {
        fetching: false,
        users: action.users
      }
    default:
      return state;
  }
};

const store = Redux.createStore(
  asyncDataReducer,
  Redux.applyMiddleware(ReduxThunk.default)
);
//// ENDCODE
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Redux with react:
-----------------
React and Redux are separate technologies.

To use react with redux, we create a single store for the entire app.
React components subscribe only to pieces of data in the store that are relevant to them.
React components dispatch actions to trigger store updates.

In react, components can have their own local state, but if the app is complex it's better to use Redux with it.
But some components may have local states specific only to them.

To use react with redux, we need 'react-redux' package.
This package gives us ways to pass state and dispatch to our React components as props.

Let's assume we have a component like this:
//// JSX
class DisplayMessages extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      input: '',
      messages: []
    }

    this.handleChange = this.handleChange.bind(this);
    this.submitMessage = this.submitMessage.bind(this);
  }

  handleChange(event) {
    this.setState({
      input: event.target.value
    });
  }

  submitMessage() {
    const { input, messages } = this.state;
    this.setState({
      input: '',
      messages: [ ...messages, input]
    });
  }
  // add handleChange() and submitMessage() methods here

  render() {
    const { input, messages } = this.state;
    return (
      <div>
        <h2>Type in a new Message:</h2>
        {/* render an input, button, and ul here */ }        
        <input type="text"
          onChange={this.handleChange}
          value={input}
          >
        </input>
        <button
          onClick={this.submitMessage}
          >Enter</button>
        <ul>
          {messages.map(item => 
            <li>{item}</li>
          )}
        </ul>

        { /* change code above this line */ }
      </div>
    );
  }
};
//// ENDCODE

We want to change it, we want move the logic it's performing locally in its state to the redux state.
Our app adds messages from the user to a list.

So, we'll need to:
- Define an action type 'ADD'.
- Define an action creator that creates this action to add a message.
- Pass the message to this action creator to be included in the created action.
- Create a reducer that handles the state for the messages.
- Create a redux store and pass it the reducer we created.

Doing all the steps:
//// Redux
const ADD = 'ADD';

const addMessage = (message) => {
  return {
    type: ADD,
    message: message
  };
}

function messageReducer(state = [], action) {
  if(action.type === ADD) {
    let newState = [...state, action.message];
    return newState;
  }
  return state;
}

const store = Redux.createStore(messageReducer);
//// ENDCODE

Now we need to give React access to the redux store and the actions.
This is done by 'react-redux' package.
Mainly we have two key features: 'provider' and 'connect'.

1)Provider: 
-----------
This is a wrapper component that wraps our entire react app, this gives us access to 'store' and 'dispatch' throughout 
our component tree.
It takes two props: the store, and child components of our app.
Example:
class AppWrapper extends React.Component {
  render() {
    return(
      <Provider store={store}>
        <DisplayMessages/>
      </Provider>
    );
  }
};

Now, our React components have access to 'state' and 'dispatch', but we must specify which actions we want so that 
each components only accesses the part of state it needs.
This is done by two functions:
- mapStateToProps().
- mapDispatchToProps().

In these functions we declare which pieces of state we want to access and which actions creators we need to be able to dispatch.

Then, the objects returned from both functions are passed as props to our components.

Behind the scenes, React Redux uses store.subscribe to implement mapStateToProps, and store.dispatch to implement mapDispatchToProps.

mapStateToProps:
- returns object that represents state.
//// Redux
const state = [];

function mapStateToProps(state) {
  return {
    messages: state
  }
}
//// ENDCODE

mapDispatchToProps:
- takes a function 'dispatch' as argument.
- returns object that has keys and values.
- each key is mapped to a function that takes relevant data as arguments.
- that function calls 'dispatch' and passes the appropriate action creator and data to it.
//// Redux
const addMessage = (message) => {
  return {
    type: 'ADD',
    message: message
  }
};

function mapDispatchToProps(dispatch) {
  return {
    submitNewMessage: (message) => 
      dispatch(addMessage(message))
  };
}
//// ENDCODE

2) connect:
-----------
So now we made our React component, Redux store, actions, creators, reducers, wrapper component i.e. provider, 
and the two functions we need to connect React to Redux.
So, we'll connect React to Redux.

The 'connect' method does that.
It can take two optional arguments 'mapStateToProps' and 'mapDispatchToProps', this is because some components 
may need to read the state but not dispatch any actions, or vice versa.

Syntax:
- connect(mapStateToProps, mapDispatchToProps)(ourReactComponent)
- connect(null, null)(ourReactComponent)

It sort of returns a function that gets immediately called and passed our component as an argument.

//// React-Redux
const addMessage = (message) => {
  ...
};

const mapStateToProps = (state) => {
  ...
};

const mapDispatchToProps = (dispatch) => {
  ...
};

class OurComponent extends React.Component {
  constructor(props) {
    ...
  }
  render() {
    ...
  }
};

const connect = ReactRedux.connect;
const ConnectedComponent = connect(mapStateToProps, mapDispatchToProps)(OurComponent);
//// ENDCODE

Usually, components that are connected to Redux are not presentational components (components responsible only for UI).
Components connected to Redux usually dispatch actions and pass store state to child components.

Our example so far:
//// React-Redux
// Redux:
const ADD = 'ADD';

const addMessage = (message) => {
  return {
    type: ADD,
    message: message
  }
};

const messageReducer = (state = [], action) => {
  switch (action.type) {
    case ADD:
      return [
        ...state,
        action.message
      ];
    default:
      return state;
  }
};

const store = Redux.createStore(messageReducer);

// React:
class Presentational extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      input: '',
      messages: []
    }
    this.handleChange = this.handleChange.bind(this);
    this.submitMessage = this.submitMessage.bind(this);
  }
  handleChange(event) {
    this.setState({
      input: event.target.value
    });
  }
  submitMessage() {
    const currentMessage = this.state.input;
    this.setState({
      input: '',
      messages: this.state.messages.concat(currentMessage)
    });
  }
  render() {
    return (
      <div>
        <h2>Type in a new Message:</h2>
        <input
          value={this.state.input}
          onChange={this.handleChange}/><br/>
        <button onClick={this.submitMessage}>Submit</button>
        <ul>
          {this.state.messages.map( (message, idx) => {
              return (
                 <li key={idx}>{message}</li>
              )
            })
          }
        </ul>
      </div>
    );
  }
};

// React-Redux:
const mapStateToProps = (state) => {
  return { messages: state }
};

const mapDispatchToProps = (dispatch) => {
  return {
    submitNewMessage: (newMessage) => {
       dispatch(addMessage(newMessage))
    }
  }
};

const Provider = ReactRedux.Provider;
const connect = ReactRedux.connect;

const Container = connect(mapStateToProps, mapDispatchToProps)(Presentational);

class AppWrapper extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <Provider store={store}>
        <Container />
      </Provider>
    );
  }
};
//// ENDCODE

Extracting local state into Redux:
----------------------------------
The final step is extracting the local state out of our components into Redux.

From the last example, we'll:
- remove message property.
- modify submitMessage() so that it dispatches a new action submitNewMessage() from this.props.
- modify the render() function so that it maps over the messages list from this.props, not from state.

//// React-Redux
// Redux:
const ADD = 'ADD';

const addMessage = (message) => {
  return {
    type: ADD,
    message: message
  }
};

const messageReducer = (state = [], action) => {
  switch (action.type) {
    case ADD:
      return [
        ...state,
        action.message
      ];
    default:
      return state;
  }
};

const store = Redux.createStore(messageReducer);

// React:
const Provider = ReactRedux.Provider;
const connect = ReactRedux.connect;

class Presentational extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      input: ''
    }

    this.handleChange = this.handleChange.bind(this);
    this.submitMessage = this.submitMessage.bind(this);
  }

  handleChange(event) {
    this.setState({
      input: event.target.value
    });
  }

  submitMessage() {
    const { submitNewMessage } = this.props;
    submitNewMessage(this.state.input);

    this.setState({
      input: ''
    })
  }

  render() {
    return (
      <div>
        <h2>Type in a new Message:</h2>
        <input
          value={this.state.input}
          onChange={this.handleChange}/><br/>
        <button onClick={this.submitMessage}>Submit</button>
        <ul>
          {this.props.messages.map( (message, idx) => {
              return (
                 <li key={idx}>{message}</li>
              )
            })
          }
        </ul>
      </div>
    );
  }
};
// Change code above this line

const mapStateToProps = (state) => {
  return {messages: state}
};

const mapDispatchToProps = (dispatch) => {
  return {
    submitNewMessage: (message) => {
      dispatch(addMessage(message))
    }
  }
};

const Container = connect(mapStateToProps, mapDispatchToProps)(Presentational);

class AppWrapper extends React.Component {
  render() {
    return (
      <Provider store={store}>
        <Container/>
      </Provider>
    );
  }
};
//// ENDCODE

And we're Done ^_^
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------------