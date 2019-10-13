# Spring Notes Collected From StackOverFlow and others

## Spring Features and Architecture
### Q: Proxy cannot be cast to CLASS?
Spring uses JDK dynamic proxies by default ($Proxy58 is one of them), that can only proxy interfaces. This means that the dynamically created type $Proxy58 will implement one or more of the interfaces implemented by the wrapped/target class (UserDao), but it won't be an actual subclass of it. That's basically why you can cast the userDao bean to the Dao interface, but not to the UserDao class.

You can use <tx:annotation-driven proxy-target-class="true"/> to instruct Spring to use CGLIB proxies that are actual subclasses of the proxied class, but I think it's better practice to program against interfaces. If you need to access some methods from the proxied class which are not declared in one of it's interfaces, you should ask yourself first, why this is the case?
(Also, in your code above there are no new methods introduced in UserDao, so there is no point in casting the bean to this concrete implementation type anyway.)

See more about different proxying mechanisms in the official Spring reference.

JDK Dynamic Proxies: https://bit.ly/2klojwy
Proxying Mechanisms in Spring: https://bit.ly/2lRTikq7 