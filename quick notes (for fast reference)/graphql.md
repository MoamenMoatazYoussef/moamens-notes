GraphQL:
========
GraphQL is basically a specification built around HTTP, it's like what comes after REST.

Assume we have an app that has a database that has two entities: Book, Author. Each entity has like 15 fields.

What if we want to get all authors' names and all the books by each author, also books' names.

How will REST and GraphQL solve this?

REST solution will be two endpoints:
	- access a list of authors using /authers, then
	- use each author's id to get all the books by that author using /authors/:id/books. (i.e. we'll call that for each author from the retrieved data in the first call)
	
GraphQL solution would be this:
	- A single query where we tell it what exactly we want:
		- A list of all authors:
			- name
			- Books: 
				- name
	- All that is done with one call using /graphql

GraphQL does all what REST does, and it's much easier for frontend to use, and much more lightweight.

Getting started:
----------------
We'll create a nodejs and express app that uses GraphQL:
- Make a folder, cd into it.
- npm init
- In package.json: change the "main" in the package.json to server.js.
- npm i express express-graphql graphql
- npm i -D nodemon
- Add this script: "devStart": "nodemon server.js"
- Create the server.js file:
``` js
const express = require("express");
const app = express();

app.listen(5000, () => console.log("Server is running"));
```
- npm run devStart
- Go to the browser tab that opened.
- Log output should work :)

Adding graphql:
- Change server.js:
``` js
// previous code before the app.listen
const expressGraphQL = require("express-graphql");

app.use("/graphql", expressGraphQL({
	graphuql: true
}));

// rest of the app
```
- Check the browser, should say that it needs a Schema

Note: graphiql is a property that provides a nice GUI to use instead of using Postman or curl.
Note: graphql is strongly typed, so you need to import object types from graphql in order to use graphql.

Defining a schema:
- Change server.js:
``` js
// previous requires
const { GraphQLSchema, GraphQLObjectType, GraphQLString } = require('graphql');

const schema = new GraphQLSchema({
	// a query is how we get the data
	query: new GraphQLObjectType({
		// the name of the entity we're getting
		name: "HelloWorld",
		
		// the fields returned by the entity HelloWorld
		fields: () => ({
			message: { 
				type: GraphQLString,
				
				// this property specifies how we'll get this field
				resolve: () => "Hello World"
			}
		})
	})
});

// rest of the code
```
- Look at the browser.
- Write this:
``` 
// by default, graphiqul will use "query", so you can just write the object
query: {
	message
}
```
- Press the play button on the GUI, which is the "Run" button.
- The output should be Hello World :)

How does GraphQL work?
- First, we define a schema, 
	- which defines a query section, 
		- which defines all the use cases for query, for example
		- the authors and books.
- In each object, we have fields
	- which represent the entity fields in the db
	- resolve, which is a function that decides 
		- what info to put in the field, and 
		- how we get it.
		- Also, it has some arguments (parent, args)
		
Making the author-book app.
- Make a list "authors", a list of objects with props:
	- id.
	- name.
- Make a list "books", a list of objects with props:
	- id.
	- name.
	- authorId: a foregin key that points to author.id.
- Create a root query scope
	- A root query is the parent query where all queries will originate fro.
	- import GraphQLList, GraphQLInt, GraphQLNonNull
	- create the schema BookType
	- create the root query and use BookType as a field type to return.
``` js
const { 
	// previous GraphQL imports
	GraphQLList,
	GraphQLInt,
	GraphQLNonNull
}

const BookType = new GraphQLObjectType({
	name: "Book",
	description: "Book type",
	fields: () => ({
		id: {
			type: GraphQLNonNull(GraphQLInt
		},
		name: {
			type: GraphQLNonNull(GraphQLString)
		},
		authorId: {
			type: GraphQLNonNull(GraphQLInt)
		}
	})
});

const RootQueryType = new GraphQLObjectType({
	name: 'Query',
	description: "This is the root query",
	fields: () => ({
		books: {
			type: new GraphQLList(BookType),
			description: "List of books",
			resolve: () => books // the list we created
		}
	})
})

const schema = new GraphQLSchema({
	query: RootQueryType
});

// the rest of the code
```
- Refresh the page, see the output :)
- Change the query in graphiql to:
```
{
	books: {
		name,
		authorId
	}
}
```
- Run it, see the output :)

What if we want to get the author.name?
- In BookType, add the field Author, of type AuthorType.

``` js
// Previous imports and BookType declaration 

const BookType = new GraphQLObjectType({
	name: "Book",
	description: "Book type",
	fields: () => ({
		id: {
			type: GraphQLNonNull(GraphQLInt
		},
		name: {
			type: GraphQLNonNull(GraphQLString)
		},
		authorId: {
			type: GraphQLNonNull(GraphQLInt)
		},
		author: {
			type: AuthorType,
			resolve: (book) => authors.find(author => author.id === book.authorId)
		}
	})
});


const AuthorType = new GraphQLObjectType({
	name: "Author",
	description: "Author type",
	fields: () => ({
		id: {
			type: GraphQLNonNull(GraphQLInt
		},
		name: {
			type: GraphQLNonNull(GraphQLString)
		}
	})
});

const RootQueryType = new GraphQLObjectType({
	name: 'Query',
	description: "This is the root query",
	fields: () => ({
		books: {
			type: new GraphQLList(BookType),
			description: "List of books",
			resolve: () => books // the list we created
		}
	})
})

const schema = new GraphQLSchema({
	query: RootQueryType
});

// the rest of the code
```
- Now, run the query:
```
{
	books: {
		id,
		author: {
			name
		}
	}
}
```
- Run it, see the output :)

GraphQL makes queries kind of like a tree that can query down to more and more sections of our app, starting with the Root Query.

Adding a query for Author:
- In the RootQuery, add "authors" field, type: list of AuthorType, change the description and resolve accordingly.

Also, do the same work to make the app able to get a query where we can get authors names and books by each author i.e. 3aks el query el fo2.

What if we want to pass arguments to each GraphQl methods?
Simple, we pass arguments.

For example, let's add a query to get ONE book:
- In the root query, add a field "book", of type BookType.
- In the resolve method, add the arguments "parent", "args".
- Before resolve, add a new property "args": {
		id: {
			type: GraphQLInt
		}
	}
- Change the resolve so that it uses the args to find a book by its id.
``` js
const RootQueryType = new GraphQLObjectType({
	name: 'Query',
	description: "This is the root query",
	fields: () => ({
		book: {
			type: BookType,
			description: "A single book",
			args: {
				id: { type: GraphQLInt }
			},
			resolve: (parent, args) => books.find(book => book.id === args.id)
		}
		// books field
		// authors fields
	})
})
```
- Use graphiql to pass an argument of 1 (the id of the book), see the result.
- Try to get the author of that book :)

That's the power of GraphQL, we don't have to make new routes, we only create a single Query field, and once it's defined, we can reuse it.
- Do the same to get a single author by id :)

Why does "fields" return a function, not an object?
- Because BookType references AuthorType and vice versa.
- So, returning a function will make it possible to execute it after BOTH AuthorType and BookType are defined.

Modifying data using GraphQL:
-----------------------------
Mutations: this is basically the equivalent of POST, PUT, DELETE, etc.

- Create a new object type called RootMutationType.
``` js
const RootMutationType = new GraphQLObjectType({
	name: "Mutation",
	description: "Root mutation",
	fields: () => ({
		// here we'll define the OPERATIONS we'll use, for example:
		addBook: {
			// the return type
			type: BookType,
			description: "Add a book",
			
			// we need to pass args, which will be the object we're adding i.e. the book.
			args: {
				name: { type: GraphQLNonNull(GraphQLString) },
				authorId: { type: GraphQLNonNull(GraphQLInt) }
			},
			resolve => (parent, args) => {
				const book = {
					// the id field is usually autogenerated by the db (AUTOINCREMENT)
					id: books.length + 1,
					name: args.name,
					authorId: args.authorId
				};
				
				books.push(book);
				return book;
			}
		}
	
	})
});
```
- Add a "mutation" prop to the schema, give it this type.
- Now, run this query in graphiql:
```
mutation {
	addBook(name: "New book", authorId: 2): {
		name,
		id
	}
}
```
- See the output :)
- Query the books and get their id and name, check the new book :)

Do the same for Author.

