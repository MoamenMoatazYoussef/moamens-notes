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

**What does AOP have to do with Spring?** <br/>
