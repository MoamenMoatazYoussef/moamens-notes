# Cypress

## Intro
A frontend test automation tool, like Selenium.
It depends on Nodejs, and it bundles jQuery.


## How it works
### Architecture
Check this: https://s3.amazonaws.com/media-p.slid.es/uploads/679820/images/5539109/pasted-from-clipboard.png

There is a middleware "browser driver", it issues commands to the REAL browser, but it still runs INSIDE the browser i.e. you have access to all the browser's features and DOM.

### Code
Cypress is similar to jQuery:
``` js
//jQuery
$('.selector');
cy.get('.selector')
//cypress
```
It also supports chaining.
In fact, cypress bundles jQuery and exposes many of its DOM methods.

But cypress doesn't return the element synchronously like jQuery, so we can't receive it in a variable normally. (So we have to use .then() )

Cypress wraps DOM queries with robust retry-and-timeout logic. If cypress doesn't find any matching DOM elements, it retries until:
- An element is found, or
- A timeout is reached: Here cypress halts and the test fails.

Querying by text content: ```cy.contains("this text")``` <br/>

## How to use Cypress
### Writing a basic test
Cypress comes by default with Mocha js testing framework, cypress needs any js testing framework and you need to write your tests to conform to the testing framework you're using i.e. use ```describe```, ```it```, etc.
```
describe("Test 1", () => {
    it("Does this", () => {
        
    });
});
```
Now let's write a simple test that opens a specific URL.
```
describe("Test 1", () => {
    it("Does this", () => {
        // This is a global object to invoke any Cypress command
        // no need to declare an instance, just use it directly
        cy.visit("http://www.google.com");
    });
});
```
Now, go to cypress test runner, refresh, run your test. (From the GUI or you can run it from the cmd, check the docs)

### Cypress folder structure
- cypress.json: the cypress configurations file. If it's empty or just has a ```{}``` this means it's using default options provided by Cypress, if you declare any configuration it will override the default property i.e. it merges whatever is written here with the default options, so what you write here overrides their corresponding properties in the defaults.
- Cypresss/
	- fixtures/: This is to store test data, we don't hardcode test data, we read it from files, these files are stored 	here.
	- integration/
		- examples/: We write our test cases here.
	- plugins/: This contains some plugins or customized options for the browser, so before invoking the browser, we'll 	tell the browser to use the config specified here.
	- support/: 
		- command.js: here we can write customized commands.
		- index.js
	- videos/: Here, some videos are made by Cypress that showcase the testing procedure, and are saved here.
	
## Code
### Locators & Selectors
Cypress only supports css selectors because cypress bundles jQuery and uses its selectors.
``` css
#id-name
.class-name
tagname.class-name
tagname[attribute=value]
```

### Getting an element and altering it:
``` js
describe('Test 1', () => {
  it('Does this', () => {
    cy.visit('http://www.google.com');
    cy.get('.gLFyf').type("google");
  });
});
```

### Finding an element within a parent element
``` js
cy.get(".parent-selector").find('.child-selector');
```
### Find the nth child of a class
Example n = 2
``` js
cy.get(".selector").eq(2);
```

### Checking that the text content of an element contains a particular string
``` js
cy.get(".selector").contains("my string");
```


