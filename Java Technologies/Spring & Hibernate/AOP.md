# Aspect-Oriented Programming
Or, AOP
## Table of Contents

## Tips

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
- Aspect: module of code for a cruss-cutting logic.
- Aspect: what action is taken, when should it be applied.
- Join point: points where we apply AOP code.
- Pointcut: expressions for where advices should be applied.
- Advising an object: it means applying AOP code (aspects) on target objects (business classes, etc.), when that happens, the target object is called "Advised object".
- Weaving: Connecting Aspects to target objects to create an "advised" object, an object that's layered with AOP code .
    - Compile-time weaving.
    - Load-time weaving.
    - Run-time weaving.

**Advice types** <br/>
- Before advice: run before the AOP method.
- After finally advice: in the finally block.
- After returning: run after successful execution of a method.
- After throwing: run after failed execution of a method (exception thrown).
- Around: before and after execution of a method.

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
		System.out.println();
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
- 