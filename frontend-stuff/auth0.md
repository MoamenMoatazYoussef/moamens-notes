Auth0 with React:
=================
How authentication is handled in web apps:
- Client makes GET request for a login page.
- Server accepts GET request and serves the login page.
- User fills form, client sends POST request with data.
- Server gets data, validates against DB, and retrieves user info.
- Server serves back the home page of the user.
- Server also sends a cookie, which is passed along with each future client request, so that the server can easily authenticate each request by finding a local session with a matching ID

Problems of that in SPAs:
- Cookies don't work well with SPAs.
- If we have a server with an SPA, it should be stateless

That's why there's OAuth: Basically, an outside third-party server is used for authentication.
In OAuth, we have 4 entities in motion: 
- Client.
- (Resource) API Server.
- Authorization server: a server used to get tokens and validate access.
- (Resource) API Owner: the end user.

OAuth uses Scopes, basically a scope is a system of permissions, so when you get an access token, you are given a Scope i.e. access to some permissions.

Tokens:
- Access token to get access to a resource, controls this access, have a small lifetime.
- Refresh token: to get a new token, can be revoked, and have a longer lifetime.

Examples of tokens:
- WS-Federated
- SAML
- JWT

JWT JSON Web Token: A JSON object that has 3 main parts:
- Header: contains info like what security algorithm is used for a signature, type of token.
- Payload: contains info about user, like user ID, expiry date, issue date, etc.
- Signature: a way to ensure integrity of the JWT.

All of this is base-64-encoded, and concatenated with a "."" as delimiter, so this generates a BIG base 64 string.

Getting started:
- Go to auth0.com
- Make an account
	- tenant: your own authorization domain
- From the dashboard, create a client, choose react, and SPA.
- Go to the client's configuration
	- add callback url, for now add localhost:3000/callback

Then, go create a new react app.
- We need state management here, you can use Redux or similar, but for learning purposes, add this global stuff in index.js:
``` js
// imports and stuff

var state = {};
window.setState = newState => {
	state = Object.assign({}, state, newState);
	ReactDOM.render(<App />), document.getElementById("root"));	
};

let initialState = {
	name: "moamen"
};

window.setState(initialState);

registerServiceWorker();
``` 

- Create 3 react components that contain a div and some unique words, names of components are Main, Secret, and PageNotFound.
- Create a routing logic, you can use React Router but for learning, this dirty solution works, in index.js:
``` js
let initialState = {
	name: "moamen",
	location: location.pathname.replace(/^\/?|\/$/g, "")
}
```

- Then in App.js:
``` js
// imports and stuff
function App(props) {
  let mainComponent = "";
  switch (props.location) {
    case "":
      mainComponent = <Main />;
      break;
    case "":
      mainComponent = <Secret />;
      break;
    default:
      mainComponent = <PageNotFound />
      break;
  }

  return (
    <div className="App">
      {/* other stuff */}
      {mainComponent}
    </div>
  );
}

export default App;

```

That's a basic b*tch app, now we'll specify a unique route, a Callback Route, to be used by auth0 to redirect users when they log in:
- Add a new component Callback
- Add a new route for Callback

Now, we'll actually work with auth0, we'll first install the library auth0-js using npm.
auth0-js is a wrapper for all auth0 APIs.

After installation finishes, restart the app, then do these edits:
- In the Main component, add a link to "/secret".
- Create a new file in src called auth:
``` js
import auth0 from "auth0-js";

export default class Auth {
	auth0 = new auth9.webAuth({
		domain: "copy it from the auth0 dashboard client's settings",
		clientID: "copy it from the auth0 dashboard client's settings",
		redirectUri: "http://localhost:3000/callback", // the uri to redirect to after authentication
		audience: "http://domain/user/info", // an api to get info about users
		responseType: "token id_token",
		scope: "openid" // what will be passed in our id token
	});

	constructor() {
		this.login = this.login.bind(this);
	}

	login() {
		this.auth0.authorize();
	}
};
```
- Go to index.js, import the class you created, and use it like this:
``` js
const auth = new Auth();

let initialState = {
	// previous initial state
	auth
}

```
- Go to App, pass the props to Main.
- Go to Main, add a button to the render, onClick={props.auth}.
- If you click on the button, it will redirect to Auth0, login with your user and authenticate it, then try the button again, it will redirect to Callback.

- Go to Auth, add two methods:
``` js
import auth0 from "auth0-js";

const LOGIN_SUCCESS_PAGE = "/secret";
const LOGIN_FAILURE_PAGE = "/"

export default class Auth {
	auth0 = new auth0.webAuth({
		domain: "mymz-auth.auth0.com",
		clientID: "ENWXyE2rZ642ztiVqEt7gtVgJyPAvTAP",
		redirectUri: "http://localhost:3000/callback", // the uri to redirect to after authentication
		audience: "http://mymz-auth.auth0.com/user/info", // an api to get info about users
		responseType: "token id_token",
		scope: "openid" // what will be passed in our id token
	});

	constructor() {
		this.login = this.login.bind(this);
	}

	login() {
		this.auth0.authorize();
	}

	handleAuthentication() {
		this.auth0.parseHash((error, authResults) => {
			if(authResults && authResults.accessToken && authResults.idToken) {
				let expiresAt = JSON.stringify((authResults.expiresIn) * 1000 * new Date().getTime());
				localStorage.setItem("access_token", authResults.accessToken);
				localStorage.setItem("id_token", authResults.idToken);
				localStorage.setItem("expires_at", expiresAt);
				localStorage.hash = "";
				window.location.pathname = LOGIN_SUCCESS_PAGE;
			} else if (error) {
				window.location.pathname = LOGIN_FAILURE_PAGE;
				console.log(error);
			}
		})
	}

	isAuthenticated() {
		let expiresAt = JSON.parse(localStorage.getItem("expires_at"));
		return new Date().getTime < expiresAt;
	}
};
```

- Now, we can secure the route /secret, go to App:
``` js
case "secret":
	mainComponent = props.auth.isAuthenticated ? <Secret /> : <PageNotFound />
```

- Go to Callback, import Auth.
- Add a componentDidMount (or the equivalent useEffect hook) with this logic inside it:
``` js
const auth = new Auth();
auth.handleAuthentication();
```

- If you try to go to /secret directly, you will end up in PageNotFound.
- But if you go to home, and login, you'll be authenticated and redirected to Secret.

- You can use isAuthenticated to display the Login button in App.

Logging out:
Logging out is easy, just forget the cached stuff.

- In Auth, add a logout method:
``` js
logout() {
	localStorage.removeItem("access_token");
	localStorage.removeItem("id_token");
	localStorage.removeItem("expires_at");	
	window.location.pathname = LOGIN_FAILURE_PAGE;
}

```

- Go to App, pass props to Secret.
- Add a logout button in Secret, onClick={props.auth0.logout}
Try it :)

ID tokens:
----------
ID tokens are basically a big string passed around, it's a JWT.
If we change the scope to profile, the token will have the profile info, we can use library "jwt-decode" to decode the token and get the profile.
This is useful if we want to get username, email, etc.
