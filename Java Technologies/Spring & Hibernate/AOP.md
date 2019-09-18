# Aspect-Oriented Programming
Or, AOP
## Table of Contents

## Tips
- sysout means System.out.print, I'm lazy.
- Check out Spring and Hibernate notes before this one.

## Let's get the show on the road
Remember the DAO we made in the last project?
We wrote code like this:
``` Java
public void add(Account a, String id) {
    Session session = sessionFactory.getCurrentSession();
    session.save(a);
}
```

While we're working on that code and being awesome, our boss comes to our desk and says:
Boss: "Heyy, we need to add some Logging to our DAO methods."
We: "Easy peazy lemon squeezy, we'll do that in an instant".
Boss: "Possibly more places, but we need to start ASAP and before you work on your TBS Reports."

So, we'll add the logging to the code:
``` Java
public void add(Account a, String id) {
    
    // add logging
    
    Session session = sessionFactory.getCurrentSession();
    session.save(a);
}
```

But then the boss comes over and says:
Boss: "Heyy dude, make sure the user is authorized before running the DAO methods, also ASAP."
We: "Okayyy, alright we'll do that"
``` Java
public void add(Account a, String id) {
    
    // logging code

    // security check code
    
    Session session = sessionFactory.getCurrentSession();
    session.save(a);
}
```
But then, the boss comes over and says: "Heyy, let's add that to all of our layers."
We: "Oh hell, I knew that was coming.."
So, you copy the code from DAO and paste it into other layers, and BOOM, done.
But, you don't feel confident about your code, you know.

Then the boss says: "Wow that was great work, add that to every layer in the entire system".

Then you resign xD <br/>

**So, what are the problems here?** <br/>
1. Code tangling: in the business logic method, we now have other code(logging, security) that has nothing to do with business logic.
2. Code scattering: if you're going to change or update the logic of that other code, you'll have to update EVERY FILE.

Well, that, is a problem. <br/>

**So, what solutions do I have other than what I did?**
1. Inheritance?
    - WE can add security and logging code in a parent class, then everyone inherits from it.
    - But, that requires all classes to extend it, still updating every file.
    - Also, what if they already extend from other classes (Jave doesn't support multiple inheritance).
2. Deligation?
    - We'll delegate logging and security calls to manager classes.
    - Same problem, we'll need to touch all of the files and classes.

But, in the darkest of times, comes a hero, the hero we all need:
**Aspect-Oriented Programming** <br/>

- It's a programming technique that's based on the concept of an "Aspect".
- An Aspect that encapsulates Cross-cutting logic (cross-cutting logic means basic infrastructure code that all apps need).
- An aspect is a class that can be applied to different parts of your projects based on your configuration.
- It's liker layers put on top of your classes.

What does AOP help us achieve?
- Proxy design pattern: main app calls the target object's methods.
    - The main app doesn't know anything about AOP, it just makes a method call.
    - THAT phone call is not directly going to the target object.
    - It's going through some "hackers", but good hackers.
    - These are the AOP code.
    - This means, we don't have to worry about AOP at all while writing our business logic code.


## Practical Stuff
**AOP Terminology** <br/>
- Aspect: A class that contains this cross-cutting logic.
- Advice: A method inside an aspect that has some cross-cutting logic and executes at specific points of the program, an aspect is a class that contains methods, these methods are "Advices".
- Join point: points where we run the AOP code.
- Pointcut: expressions for where advices should be applied.
- Advising an object: it means assigning AOP code (methods inside aspects i.e. advices) on target objects (other classes and methods) so that advices run at some time relative to target objects (Before them, after them, etc.), when that happens, the target object is called "Advised object".
- Weaving: Connecting Aspects to target objects to create an "advised" object, an object that's layered with AOP code .
    - Compile-time weaving.
    - Load-time weaving.
    - Run-time weaving.

**Advice types** <br/>
An advice is a method inside an Aspect(A class used for AOP).
Since we can make these methods -these advices- run at a time relative to other classes and methods, we can classify these methods according to when do they run:
- Before advice: an advice method that runs before the target object (method or class) runs.
- After finally advice: an advice method that runs in the finally block.
- After returning: an advice method that runs after successful execution of a method.
- After throwing: an advice method that runs after failed execution of a method (exception thrown).
- Around: an advice method that runs before and after execution of a method.

**AOP Frameworks for Java** <br/>
- AspectJ: 
    - The OG(original gangsta) of AOP.
    - Provides complete AOP support.
    - Supports join points at class level, method level, constructor level, and field level.
    - Supports the three types of code weaving.
    - Works with any POJO (Plain Old Java Object).
    
- Spring AOP: 
    - Spring uses AOP in the background.
    - Supports user-defined AOP stuff.
    - Uses Proxy pattern.
    - Supports method-level join points only.
    - Uses run-time weaving only, which causes a small performance cost.

**Comparison between the two** <br/>
- AspectJ is much more complex than Spring AOP.
- But AspectJ is very fast and more powerful.
- AspectJ supports full AOP.
- Spring supports most of AOP's features.

**So what do I choose?** <br/>
- Start with Spring AOP.
- If things require really complex AOP programming, move to AspectJ.

**Resources** <br/>
- Spring AOP: www.spring.io, the documentation of Spring.
- AspectJ: "AspectJ in Action" Book.
- AOP as a concept and its use cases: "Aspect-Oriented Development with Use Cases" Book.

**What will we do here?** <br/>
- We'll cover Spring AOP.
- Create aspects.
- Develop Advices with all their types.
- Create pointcut expressions.
- Apply this to our Spring + Hibernate CRM project.

## AOP Programming Overview
### Advices
**@BeforeAdvice** <br/>
- The Before advices is code that runs before a certain method runs.
- So, our main app will call a method.
- We want our own custom code execute BEFORE it.

**When and why do we use this? Use cases** <br/>
- Logging, security, transactions (for example the @Transactional spring annotation we used before).
- Audit logging
- API management, for example Who called a method, How many times was it called, what is the peak time of calling this method, etc.

**How do we do that?** <br/>
We'll have a main app, this main app will use a DAO to call a method.
We'll inject some AOP before the method executes.

Steps:
1. Create target object, the DAO.
2. Create a Spring Java Config class.
3. Create main app.
4. Create an aspect using an @Before annotation.

**Best Practices with AOP** <br/>
That code will execute EVERY TIME this method is called, so:
- Keep the code small and fast.
- Do not perform slow operations.
- Get in and get out as quickly as possible.

### The worst part of any framework - Setting Up Spring AOP 
Eclipse:
- Remember spring-demo-one or spring-demo-annotations? Copy one of them, paste it, call it "spring-demo-aop".
- Delete all src files and all packages.
- We need the AspectJ jar files since Spring AOP uses it, download it:
    + don't choose the beta version, choose the latest stable version
    + in the next screen download the JAR file from the Files tab:
    + www.luv2code.com/download-aspectjweaver
- Copy it into your lib directory.
- Add all JARS in the lib directory (spring JARs and the AspectJ JAR) to the build path of the project.

Done baby B) <br/>

### Now, to write some CODE!
**Step 1: Create the target object** <br/>
- Create a new package: com.luv2code.aopdemo
- Create another package: com.luv2code.aopdemo.dao
- Create the target object, the DAO class, annotate it with @Component, add one method, inside it sysout anything.
``` java
@Component
public class AccountDAO {
	public void addAccount() {
		System.out.println("I'm in the main method.");
	}
}
```

**Step 2: Create the Spring Config class** <br/>
- Create a Spring Confige java class.
- Annotate it with @Configuration and @ComponentScan, passing in the package.
- Annotate it with @EnableAspectJAutoProxy, the new component that enables AOP to execute in a proxy design pattern manner.
``` Java
@Configuration
@EnableAspectJAutoProxy
@ComponentScan("com.luv2code.aopdemo")
public class DemoConfig {

}
```

**Step 3: Create the main app** <br/>
- We did that before xD
``` Java
public class MainDemoApp {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = 
				new AnnotationConfigApplicationContext(DemoConfig.class);
		
		AccountDAO theAccountDAO = context.getBean("accountDAO", AccountDAO.class);
		
		theAccountDAO.addAccount();
		
		context.close();
	}

}
```

**Step 4: Create the AOP code** <br/>
- The AOP code is called an "Aspect",
- Create a new package: com.luv2code.aopdemo.aspect
- In it, create a new class: MyDemoLoggingAspect.
- Annotate it with @Aspect and @Component (So that Spring can pick it up during component-scanning).
- Inside the class, write a method, inside it write a sysout or something.
- Annotate that method with @Before("execution(public void addAccount())"):
    + Before: means that this method will run Before a specific event. That event is what we pass as argument.
    + The word "Execution" means: execute the method Before any execution of what we'll pass inside.
    + Then, we pass the name of our method.
    + So, this annotation means: Execute this function Before every time the function addAccount executes.
    + That function, is called an "Advice".
``` java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class MyDemoLoggingAspect {

	@Before("execution(public void addAccount())")
	public void MyAdvice() {
		System.out.println("I'm in the Advice now.");
	}
}
```

Now, run the main app, observe the output.
Now this is spying on the code :) <br/>
Try calling the method many times in the main app, see the output and watch the advice method run every time the main method is called, but before it.

## AOP Pointcut Expressions
Remember when we wrote that @Before annotation, and inside it we wrote some stuff that defined WHEN the advice method is applied?
This stuff is an expression called a Pointcut Expression, it's like a condition that, when met, the advice method is applied.

Pointcut Expressions are a language specific to AspectJ, and Spring AOP uses it.

### Pointcut Expressions: basic Method Matching
**How do we write Pointcut Expressions** <br/>
Pointcut Expressions have a specific syntax that consists of Patterns.
All of the patterns must be matched for the aspect to run. 
For example we have the Execution expression:
```
execution(modifiers-pattern? return-type-pattern declaring-type-pattern? method-name-pattern(param-pattern) throws-pattern?)
```
All patterns that have "?" are optional.
What do those patterns mean?
- modifiers-pattern: match on a specific modifier (if we have two methods of the same name and everything but one is public the other is private, whatever is specified in the modifiers pattern will ONLY have the aspect run before it(or after or whatever, according to the type of aspect))
- return-type-pattern: what's the return type to match?
- declaring-type-pattern: what's the class name of the type to match?
- method-name-pattern: what's the name of the method?
- param-pattern: what are the parameters that should be there for the AOP to execute?
- throws-pattern: does the method throw a given exception?

Patterns can get wildcards * i.e. that pattern matches with everything.

**Some Examples** <br/>
Example:
``` Java
@Before("execution(public void com.luv2code.aopdemo.dao.AccountDAO.addAccount())")
```
- Modifiers pattern: public
- Return type pattern: void
- Declaring type: com.luv2code.aopdemo.dao.AccountDAO.
- Name: addAccount.
- parameters and throws patterns: nothing.

This means:
Execute this aspect before every execution of:
- the public function 
- called "addAccount"
- that returns void
- that's declared inside com.luv2code.aopdemo.dao.AccountDAO
- that takes NO parameters
- and throws NO expressions.

Example:
``` Java
@Before("execution(public void addAccount())")
```
- Modifiers pattern: public
- Return type pattern: void
- Declaring type: nothing (it's an optional pattern)
- Name: addAccount.
- parameters and throws patterns: nothing.

This means:
Execute this aspect before every execution of:
- the public function 
- called "addAccount"
- that returns void
- that's declared inside ANY CLASS (see the difference here?)
- that takes NO parameters
- and throws NO expressions.

Example:
``` Java
@Before("execution(public void add*())")
```
- Modifiers pattern: public
- Return type pattern: void
- Declaring type: nothing (it's an optional pattern)
- Name: any name that starts with "add" (interesting..)
- parameters and throws patterns: nothing.

This means:
Execute this aspect before every execution of:
- the public function
- whose name starts with "add" (add, addAccount, addThisNumber, addBlaBla, etc.)
- that returns void
- that's declared inside ANY CLASS
- that takes NO parameters
- and throws NO expressions.

Example:
``` Java
@Before("execution(* * add*())")
```
- Modifiers pattern: any modifier
- Return type pattern: any return type
- Declaring type: nothing (it's an optional pattern)
- Name any name that starts with "add"
- parameters and throws patterns: nothing.

This means:
Execute this aspect before every execution of:
- any function
- whose name starts with "add"
- with any return type
- that's declared inside ANY CLASS
- that takes NO parameters
- and throws NO expressions.

**Let's apply that in code** <br/>
In the previous code, this is our pointcut expression:
``` Java
@Before("execution(public void addAccount())")
```
**In the code, try these** <br/>
+ **Case 1:** In the expression, change the name of the method from addAccount to updateAccout (a method we don't have).
+ **Case 2:** Create a new class MembershipDAO, and inside it define a method: public void addAccount(), then call it and the old addAccount method in the main app.
+ **Case 3:** add a declaration type in the pointcut expression i.e. change it to this:
``` Java
@Before("execution(public void com.luv2code.aopdemo.dao.AccountDAO.addAccount())")
``` 
**Hint:** select the class name AccountDAO, choose Copy Qualified Name, paste it before the method name and add a dot after it.
+ **Case 4:** use the wildcard, change the name of the method to "add*" (don't forget to remove the FQN of the class name), and change the name of the addAccountin MembershipDAO to "addSillyMember" for example.
+ **Case 5:** change the membershipDAO method's modifier to default, then in the expression, change the modifier to the wildcard * i.e. do this:
``` Java
@Before("execution(* void add*())")
```
+ **Case 6:** change the return type from void to the wildcard "*" i.e.
``` Java
@Before("execution(* * add*())")
```

**Run the code each time and observe the output** <br/>
+ **Case 1 output:** the AOP code won't execute ever, because the method updateAccount doesn't exist in our code.
+ **Case 2 output:** the AOP code will execute before both method calls, because we don't define a declaration type for our method.
+ **Case 3 output:** the AOP code will execute only before the first method call (the old addAccount), because this is the one declared in the declaration type we specified, the other is in MembershipDAO.
+ **Case 4 output:** the AOP code will execute before both method calls, because it will match any method whose name has "add" in its start, regardless of what follows.
+ **Case 5 output:** the AOP code will execute before both method calls, because it will match any method whose name has "add" in its start, regardless of what follows or what's the modifier of the method.
**Case 6 output:** the AOP code will execute before both method calls, because it will match any method whose name has "add" in its start, regardless of what follows, the modifier, or the return type.

### Pointcut Expressions: Parameter Matching
We can specify specific parameters to match, we can use some wildcards like:
- (): match method with no arguments.
- (*): match method with *one* or more arguments.
- (..): match method with *zero* or more arguments. (i.e. anything)

Or, we can specify arguments by passing the Fully Qualified Name of the parameter type, for example:
``` Java
@Before("execution(* * addAccount(com.luv2code.aopdemo.Account))")
```
This means, match with:
- a method of any return type,
- and any modifier,
- whose name is addAccount,
- which takes one parameter of type Account

Notice that we wrote the FQN of the parameter type.
