# Errors Collected From StackOverFlow and others

## Spring
### Spring: Q: Proxy cannot be cast to CLASS?
Spring uses JDK dynamic proxies by default ($Proxy58 is one of them), that can only proxy interfaces. This means that the dynamically created type $Proxy58 will implement one or more of the interfaces implemented by the wrapped/target class (UserDao), but it won't be an actual subclass of it. That's basically why you can cast the userDao bean to the Dao interface, but not to the UserDao class.

You can use <tx:annotation-driven proxy-target-class="true"/> to instruct Spring to use CGLIB proxies that are actual subclasses of the proxied class, but I think it's better practice to program against interfaces. If you need to access some methods from the proxied class which are not declared in one of it's interfaces, you should ask yourself first, why this is the case?
(Also, in your code above there are no new methods introduced in UserDao, so there is no point in casting the bean to this concrete implementation type anyway.)

See more about different proxying mechanisms in the official Spring reference.

JDK Dynamic Proxies: https://bit.ly/2klojwy
Proxying Mechanisms in Spring: https://bit.ly/2lRTikq7 

### SQL with Java: ConstraintViolationException: Cannot add or update a child row: a foreign key constraint fails
#### Explanation
You're trying to change data in a SQL table, this SQL table may be:
- A table for an entity, that entity has a relation with another entity.
- A join table for a relation between two entities.

In the first case, either this entity or the other entity has, in its table, a column for a Foreign Key that represents the relation between the entities, for example entity_1's table has an additional column called entity_2_id, it's a foreign key that references entity_2's table, so that we can deduce the relation between the two.

In the second case, there are two entities that have a many-to-many relation, this relation is expressed not by having foreign key columns in one of their tables, but by having a THIRD table with two columns, each is a foreign key column that references one of the tables.

In both cases, there is usually a constraint applied on the database, it is: *the tables must be consistent*, this means that because these foreign key columns represent the relation between two entities, therefore:
+ *we can't have a foreign key entry in entity_1's table, for entity_2, that doesn't exist in entity_2's table*.
+ For example, if entity_2 has entries 1, 2, 3, we can't have an entry of 4 in the entity_2_id column in entity_1's table, because entity_2_id references the id column of entity_2, which doesn't have 4 in it.

The exception **ConstraintViolationException** is thrown when your code is trying to do some operation on the database that breaks this constraint, for example:
- Inserting an entry in a join table or a foreign key column that doesn't exist in the referenced table(s).
- Deleting an entry from an *entity* table whose key is referenced in a join table or a foreign key column in another table, because now that entity doesn't exist so the foreign key entries break the constraint.

#### Solutions for the problem
- Check the order of inserting in the database:
    + You might be inserting in a join table/foreign key column in a referencing table before inserting the entity referenced by the entry you're adding in the join table/foreign key column in a referencing table, therefore breaking the constraint.
    + The correct sequence should be similar to this:
        - Insert first in the *entity* that doesn't have a foreign key, but is referenced by another entity.
        - Then, insert the entry that references the entity we just inserted in its respective table (join table or other entity table that has foreign key column) 
- Check the order of deletion in the database:
    + Same concept.
    + The correct sequence should be similar to this:
        - Delete first from the join table/other table with foreign key referencing the entities.
        - Then delete the entities themselves from their tables.



## React
### No 'Access-Control-Allow-Origin' header is present on the requested resource
#### Explanation
The frontend JS code of any web application can only access and request resources from the same origin the application was loaded from i.e. the backend of the same application, this is for security reasons.

If we want the frontend JS code to access resources from a different application, there is a mechanism for that, it is basically some additional HTTP headers added to frontend requests from different apps, and policies to ensure secure resource sharing, this mechansim is called *Cross-Origin Resource Sharing*, or ***CORS***.

CORS is used by browsers in APIs such as Fetch, Axios, XMLHttpRequest, etc., so browsers handle the client-side of CORS (headers and policy enforcement), but *backend developers* should handle the CORS themselves.

Articles to read:
- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Server-Side_Access_Control

#### Solutions
Check this link:
- https://bit.ly/2TBnccw