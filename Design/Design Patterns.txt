Singleton:
----------
One instance of our class.
Example: single instance of DB connection, shared among multiple objects.

class className {
	private static className obj;
	
	private className {} 
	//private to force creation
	//through the next function
	
	public static className getInstance()
	{
		if(obj == null)
			obj = new className();
		return obj;
	}
}

Here the object is not created until we call the function "Lazy instantiation".

This method is NOT thread-safe, two threads may create two instances.

class className {
	private static className obj;
	
	private className {} 
	//private to force creation
	//through the next function
	
	public static synchronized className getInstance()
	{
		if(obj == null)
			obj = new className();
		return obj;
	}
}

The synchronized keyword ensures that only one thread can execute this function, like pragma omp critical.

This is thread-safe, but is expensive in performance.

class className {
	private static className obj = new className();
	
	private className {}
	
	public static synchronized className getInstance()
	{
		return obj;
	}
}

This method is thread-safe because JVM execute the static initializer when the class is loaded.

class className {
	private volatile static className obj;
	
	private className {}
	
	public static synchronized className getInstance()
	{
		if(obj == null)
		{
			synchronized(className.class) {
				if(obj == null)
					obj = new className();
			}
		}
		return obj;
	}
}

This one is amazing because it used synchronization ONLY when creating the object, so we only acquire the lock when we initialize, not every time we call getInstance method.

volatile ensures that many threads offer obj variable correctly when it's being initialized, this reduces the overhead of synchronized calling every time.









Facade pattern:
---------------
A structural pattern.

Hide implementation of systems and provide an interface to deal with them, a Facade.

Example: you want to access menues from restaurants in a hotel, you just talk to the hotel keeper.

//This is an interface to deal with.
public interface Hotel 
{ 
    public Menus getMenus(); 
} 

//These are two restaurants that implement the interface.
public class NonVegRestaurant implements Hotel 
{ 
    public Menus getMenus() 
    { 
        NonVegMenu nv = new NonVegMenu(); 
        return nv; 
    } 
} 

public class VegRestaurant implements Hotel 
{ 
    public Menus getMenus() 
    { 
        VegMenu v = new VegMenu(); 
        return v; 
    } 
}

//And this is the facade:
public class HotelKeeper 
{ 
    public VegMenu getVegMenu() 
    { 
        VegRestaurant v = new VegRestaurant(); 
        VegMenu vegMenu = (VegMenu)v.getMenus(); 
        return vegMenu; 
    } 
      
    public NonVegMenu getNonVegMenu() 
    { 
        NonVegRestaurant v = new NonVegRestaurant(); 
        NonVegMenu NonvegMenu = (NonVegMenu)v.getMenus(); 
        return NonvegMenu; 
    } 
} 

//So, the Facade has the complex implementation, the client only needs to call functions.

public class Client 
{ 
    public static void main (String[] args) 
    { 
        HotelKeeper keeper = new HotelKeeper(); 
        VegMenu v = keeper.getVegMenu(); 
        NonVegMenu nv = keeper.getNonVegMenu();
    } 
} 

When to use:
------------
When you have a complex system that you want to expose in a simplified way.
When you want to make a comm layer over a system which is incompatible with other systems.

So it can work well with Adapter pattern.





Proxy pattern:
--------------
Proxy is a handle, or surrogate, like a checque or credit card, its a proxy for the money in our bank account. It can be used in place of it and provides means to access it.

Proxy pattern: controls and manages access to an object they protect.

Types of proxies:
-----------------
* Remote: they represent a remote object, talking to that object may involve transforming data into a format for transmission (Marshalling), that is implemented inside the proxy and hidden.

* Virtual: they provide default and instant results if the real object will take time to produce results.
	So they provide a default result, then when the real result arrives it's pushed to the client.
	
* Protection: if the client doesn't have authorization to some resource, the proxies are used to communicate.

* Smart: the proxy may contain some actions, like checking if the object is accessed or locked, etc.

Example: college internet, which has some sites closed.

//An interface which is used to connect to a host
public interface Internet 
{ 
    public void connectTo(String serverhost) throws Exception; 
} 

//This is the real object we're protecting
public class RealInternet implements Internet 
{ 
    @Override
    public void connectTo(String serverhost) 
    { 
        System.out.println("Connecting to "+ serverhost); 
    } 
} 

//This is the proxy
public class ProxyInternet implements Internet 
{ 
    private Internet internet = new RealInternet(); 
    private static List<String> bannedSites; 
      
    static
    { 
        bannedSites = new ArrayList<String>(); 
        bannedSites.add("abc.com"); 
        bannedSites.add("def.com"); 
        bannedSites.add("ijk.com"); 
        bannedSites.add("lnm.com"); 
    } 
      
    @Override
    public void connectTo(String serverhost) throws Exception 
    { 
        if(bannedSites.contains(serverhost.toLowerCase())) 
        { 
            throw new Exception("Access Denied"); 
        } 
          
        internet.connectTo(serverhost); 
    } 
  
} 

//And here's the client using the proxy to access the real object
public class Client 
{ 
    public static void main (String[] args) 
    { 
        Internet internet = new ProxyInternet(); 
        try
        { 
            internet.connectTo("geeksforgeeks.org"); 
            internet.connectTo("abc.com"); 
        } 
        catch (Exception e) 
        { 
            System.out.println(e.getMessage()); 
        } 
    } 
}

When to use it:
---------------
When you want to create a wrapper to hide the main object's complexity.
When you want security and authorization for your object.

NOTE:
-----
Adapter: gives a different interface to facilitate communication with the object.
Proxy: provides the same interface as the object but with some limitations.
Decorator: provides the same interface but enhanced and adds additional behavior at runtime.

Proxies can be chained together, like Decorator and Adapter.


Composite:
----------
We compose an object into a tree structure, so that we can ask each node to perform a task.

When dealing with trees, we need to differentiate between leafs and branches, this makes code more complex.

The solution is to use an interface that lets us treat both uniformly.

A composite is an object that composes of one or more similar objects with similar functionalities.

The concept is: we can utitilze one object just as we utilize a group of it. The operations performed on composite objects have a least common denominator relationship.

Composite:
----------
1. Component: declares the interface to access and manage a composition and its child components.
2. Leaf: defines a behavior of primitive objects.
3. Composite: stores child components and implements child-related ops.
4. Client: uses the interface to manipulate the objects.

If client requests sth from a LEAF, the request is handled.
If client requests sth from a BRANCH, the request is forwarded -with some ops- until a leaf handles it.

Example:
	An org structure with General Managers, under them Managers, under them Developers. All of them may get asked to getSalary();
	
//Here is the interface to deal with objects.
public interface Employee 
{ 
    public void showEmployeeDetails(); 
} 

//Here is the LEAF class, implementing the interface.
public class Developer implements Employee 
{ 
    private String name; 
    private long empId; 
    private String position; 
  
    public Developer(long empId, String name, String position) 
    { 
        this.empId = empId; 
        this.name = name; 
                this.position = position; 
    } 
      
    @Override
    public void showEmployeeDetails()  
    { 
        System.out.println(empId+" " +name+); 
    } 
} 

//Here is the composite
public class CompanyDirectory implements Employee 
{ 
    private List<Employee> employeeList = new ArrayList<Employee>();  //List of nodes under it
       
    @Override
    public void showEmployeeDetails()  //Here is the forwarding.
    { 
        for(Employee emp:employeeList) 
        { 
            emp.showEmployeeDetails(); 
        } 
    } 
       
    public void addEmployee(Employee emp) 
    { 
        employeeList.add(emp); 
    } 
       
    public void removeEmployee(Employee emp) 
    { 
        employeeList.remove(emp); 
    } 
} 

//Driver class
public class Company 
{ 
    public static void main (String[] args) 
    { 
        Developer dev1 = new Developer(100, "Lokesh Sharma", "Pro Developer"); 
        Developer dev2 = new Developer(101, "Vinay Sharma", "Developer"); 
        CompanyDirectory engDirectory = new CompanyDirectory(); 
        engDirectory.addEmployee(dev1); 
        engDirectory.addEmployee(dev2); 
          
        Manager man1 = new Manager(200, "Kushagra Garg", "SEO Manager"); 
        Manager man2 = new Manager(201, "Vikram Sharma ", "Kushagra's Manager"); 
          
        CompanyDirectory accDirectory = new CompanyDirectory(); 
        accDirectory.addEmployee(man1); 
        accDirectory.addEmployee(man2); 
      
        CompanyDirectory directory = new CompanyDirectory(); 
        directory.addEmployee(engDirectory); 
        directory.addEmployee(accDirectory); 
        directory.showEmployeeDetails(); 
    } 
} 

Why to use:
-----------
Less number of objects, less memory.
Reduce exec time of creating many objects, by sharing objects.

When to use:
------------
When you want clients to ignore difference between composites and leafs.
When you find that you're using many objects in the same way and have nearly identical code to handle each.


When not to use:
----------------
When you want full or partial hierarchy of objects, since it makes restricting type of components of a composite harder.
It may make the design overly general, which may lead to using runtime checks to restrict it, which is overhead.


Prototype:
----------
A creational pattern.

Why?
----
* To hide complexity of making new instances of an object.
* To reduce overhead of object creation.
* Add/remove products at runtime, more flexible.
* Instantiate complex structures (composites) or use a specific subcircuit to create complex structures.
* Reduced subclassing, factory method produces a hierarchy of creator classes, but prototype just clones objects instead of asking the factory to create a new one.

Concept:
--------
* Instead of making new instances, copy old ones.
* How the copying itself works is your thing.

Elements:
---------
1. Prototype: the object we'll clone.
2. Prototype registry: a registry that has all prototypes accessible using string e.g. map.
3. Client.

When to use:
------------
* When you want the system to be independent of how the products are created, composed, represented.
* When classes to instantiate are specified at runtime. (Dynamic loading)
* When instances of a class exist in only a few different states, just clone into the new state.

When not to use:
----------------
* If you have very few objects in the project.
* If you don't want to hide concrete classes from the client.
* Each subclass must implement the clone() operation.

Example:
--------
//Our base class
abstract class Color implements Cloneable 
{ 
      
    protected String colorName; 
    abstract void addColor();
    public Object clone() 
    { 
        Object clone = null; 
        try 
        { 
            clone = super.clone(); 
        }  
        catch (CloneNotSupportedException e)  
        { 
            e.printStackTrace(); 
        } 
        return clone; 
    } 
}

//One child of base class, a prototype
class blueColor extends Color 
{ 
    public blueColor()  
    { 
        this.colorName = "blue"; 
    } 
   
    @Override
    void addColor()  
    { 
        System.out.println("Blue color added"); 
    } 
      
}

//Another child, and a prototype
class blackColor extends Color{ 
   
    public blackColor() 
    { 
        this.colorName = "black"; 
    } 
   
    @Override
    void addColor()  
    { 
        System.out.println("Black color added"); 
    } 
}

//The prototype registry
class ColorStore { 
   
    private static Map<String, Color> colorMap = new HashMap<String, Color>();  
       
    static 
    { 
        colorMap.put("blue", new blueColor()); 
        colorMap.put("black", new blackColor()); 
    } 
       
    public static Color getColor(String colorName) 
    { 
        return (Color) colorMap.get(colorName).clone(); 
    } 
}

//Driver class
class Prototype 
{ 
    public static void main (String[] args) 
    { 
        ColorStore.getColor("blue").addColor(); 
        ColorStore.getColor("black").addColor(); 
        ColorStore.getColor("black").addColor(); 
        ColorStore.getColor("blue").addColor(); 
    } 
} 

Template design pattern:
------------------------
A behavioral pattern.

Defines an algorithm as a skeleton of ops, then leaves the details to be implemented by children.

So, an algorithm in a parent class with steps that are abstract, and its details in the child classes.

Templates are like presets.

Components:
-----------
1. Abstract class:
	- templateMethod(): 
		- should be final so that it can't be overridden.
		- makes use of operations available to run the algorithm, but decoupled from the implementation of these steps.
	- operations used in templateMethod, they should be abstract.
	
2. Concrete child class:
	- implements the operations required by templateMethod

When to use?
------------
* Commonly used in framework development.
* Helps avoid code duplication.
* Here, the main difference between this and simple polymorphic overriding is that we don't override the WHOLE thing, we just override the steps of some function that is final and can't be overridden.

Example:
--------
abstract class OrderProcessTemplate 
{ 
    public boolean isGift; 
    public abstract void doSelect(); 
    public abstract void doPayment(); 
    public final void giftWrap() 
    { 
        try
        { 
            System.out.println("Gift wrap successfull"); 
        } 
        catch (Exception e) 
        { 
            System.out.println("Gift wrap unsuccessful"); 
        } 
    }
    public abstract void doDelivery(); 
    public final void 	processOrder(boolean isGift) 
    {	//here are the steps in the fn
		//they are abstract 
        doSelect(); 
        doPayment(); 
        if (isGift) { 
            giftWrap(); 
        } 
        doDelivery(); 
    } 
} 

class NetOrder extends OrderProcessTemplate 
{ 
    @Override
    public void doSelect() 
    { 
        //code
    } 
  
    @Override
    public void doPayment() 
    { 
        //code
    } 
  
    @Override
    public void doDelivery() 
    { 
		//code
    } 
} 
--------
We can have more than one child.
-------------------------------------------------
Builder design pattern:
-----------------------
Why?
----
Separate the construction of a complex object from its representation.

So that the same construction process can construct different representations.

It's like a step by step creation of an object and in the last step the object is returned.

We aim to make the construction process generic so that we can create different representations of the same object.






----------------------------------------------
Abstract Factory:
-----------------
Creational.

Similar to factory, considered another layer of abstraction over factory.

It's essentially an interface for factories, a super-factory.

Why/when to use
---------------
* Concrete classes are isolated.
* Easy to choose between factories, the abstract factory creates a whole family of products.
* When many products are designed to work together, it's better than any app uses objects from only one family at a time, this design makes this easy to enforce.

When not to use
---------------
* if new products are added, adding new factories is hard because abstractFactory fixes the set of products it can produce, so we need to extend it too, and may change many things in other classes.

Components:
-----------
* AbstractFactory: interface for ops that create abstract objects.
* ConcreteFactory: implements these operations.
* AbstractProduct: interfaces for different products created by the factories.
* Product: what's created by each factory implementing the abstractFactory.
* Client: uses the interfaces to create products.

Steps:
------
Client creates concrete implementation of abstract factory.
Client uses interfaces to create concrete objects.
Client doesn't know or care which objects are created since it uses abstract interface for the products.

Implementation:
---------------

enum CarType 
{ 
    MICRO, LUXURY 
}

//Interface for products
 abstract class Car 
{ 
    Car(CarType model, Location location) 
    { 
        //code
    } 
   
    abstract void construct(); 
   
    CarType model = null; 
    Location location = null; 
   
    CarType getModel() 
    { 
        return model; 
    } 
   
    void setModel(CarType model) 
    { 
        this.model = model; 
    } 
   
    Location getLocation() 
    { 
        return location; 
    } 
   
    void setLocation(Location location) 
    { 
        this.location = location; 
    } 
   
    @Override
    public String toString() 
    { 
        return "CarModel - "+model + " located in "+location; 
    } 
} 

//Concrete products
class LuxuryCar extends Car 
{ 
    LuxuryCar(Location location) 
    { 
        super(CarType.LUXURY, location); 
        construct(); 
    } 
    @Override
    protected void construct() 
    { 
        System.out.println("Connecting to luxury car"); 
    } 
} 
  
class MicroCar extends Car 
{ 
    MicroCar(Location location) 
    { 
        super(CarType.MICRO, location); 
        construct(); 
    } 
    @Override
    protected void construct() 
    { 
        System.out.println("Connecting to Micro Car "); 
    } 
} 


enum Location 
{ 
  USA, INDIA 
} 
  
//Concrete factories
class INDIACarFactory 
{ 
    static Car buildCar(CarType model) 
    { 
        Car car = null; 
        switch (model) 
        { 
            case MICRO: 
                car = new MicroCar(Location.INDIA); 
                break; 
                  
            case LUXURY: 
                car = new LuxuryCar(Location.INDIA); 
                break; 
                  
                default: 
                break; 
              
        } 
        return car; 
    } 
} 

class USACarFactory 
{ 
    public static Car buildCar(CarType model) 
    { 
        Car car = null; 
        switch (model) 
        { 
            case MICRO: 
                car = new MicroCar(Location.USA); 
                break;
                  
            case LUXURY: 
                car = new LuxuryCar(Location.USA); 
                break; 
                  
                default: 
                break; 
              
        } 
        return car; 
    } 
} 

//Abstract factory
class CarFactory 
{ 
    private CarFactory() { } 
    public static Car buildCar(CarType type) 
    { 
        Car car = null;
        Location location = Location.INDIA;  
          
        switch(location) 
        { 
            case USA: 
                car = USACarFactory.buildCar(type); 
                break; 
                  
            case INDIA: 
                car = INDIACarFactory.buildCar(type); 
                break;
        } 
          
        return car; 
  
    } 
} 

//Driver class
class AbstractDesign  
{ 
    public static void main(String[] args) {
	   System.out.println(CarFactory.buildCar(CarType.MICRO)); 
       System.out.println(CarFactory.buildCar(CarType.LUXURY)); 
    } 
} 

-------------------------------------
Bridge pattern:
---------------
Structural.

It allows us to separate abstraction from implementation, so that they can be developde independently and client only access abstraction part.

Abstractions are interfaces or abstract classes, implementors are also interfaces or abstract classes.

So an abstraction contains a ref to an implementor.

Children of abstractions are "refined abstractions", and children of implementors are "concrete implementors"

Since we reference the implementor we can change it at runtime, so there's loose coupling between abstraction and implementors.

Components:
-----------
* Abstraction: the core of the pattern, it references the implementor.

* Refined abstraction: extends abstraction and takes one level deeper in detail, and hids these details from the implementors.

* Implementor: an interface for the implementation class, which can be different from the abstraction, it just is referenced by the abstraction.

* Concrete implementation: the actual implementation of the implementor.

Example:
---------
// abstraction in bridge pattern 
abstract class Vehicle { 
	//These are ref to implementors
    protected Workshop workShop1; 
    protected Workshop workShop2; 
  
    protected Vehicle(Workshop workShop1, Workshop workShop2) 
    { 
        this.workShop1 = workShop1; 
        this.workShop2 = workShop2; 
    } 
  
    abstract public void manufacture(); 
} 

//This is a refined abstraction
class Car extends Vehicle { 
    public Car(Workshop workShop1, Workshop workShop2) 
    { 
        super(workShop1, workShop2); 
    } 
  
    @Override
    public void manufacture() 
    { 
        System.out.print("Car "); 
        workShop1.work(); 
        workShop2.work(); 
    } 
} 

//This is an implementor 
interface Workshop 
{ 
    abstract public void work(); 
} 
  
// Concrete implementation 1
class Produce implements Workshop { 
    @Override
    public void work() 
    { 
        System.out.print("Produced"); 
    } 
} 

// Conrete implementation 2
class Assemble implements Workshop { 
    @Override
    public void work() 
    { 
        System.out.print(" And"); 
        System.out.println(" Assembled."); 
    } 
} 

// Driver class
class BridgePattern { 
    public static void main(String[] args) 
    { 
        Vehicle vehicle1 = new Car(new Produce(), new Assemble()); 
        vehicle1.manufacture(); 
        Vehicle vehicle2 = new Bike(new Produce(), new Assemble()); 
        vehicle2.manufacture(); 
    } 
} 

----
As we can see, we can use the abstraction and refined abstraction to create and work with different classes while using the same code, which demonstrates the loose coupling of abstraction and implementation.

Why/when to use?
----------------
Advice: composition > inheritance.

If you have many classes that are similar and inherit from a superclass, you may want to consider the bridge pattern.

We kind of separate the implementation of methods from the entities, so we can make different entities with much less code and much loose coupling.

It can be used in runtime mapping of class hierarchies.
It can be used to bind implementation in runtime like virtual functions.

-----------------------------------------------------------------------------------------------
MCV pattern:
------------
App consists of:
- Model.
- Presentation info.
- Control info.

Model: contains data, doesn't contain logic.

View: presents data to user, knows how to access data in model, but doesn't know what it means.

Controller: listens to events from user, executes reaction to these events.

Example code:
-------------
// Model
class Student  
{ 
    private String rollNo; 
    private String name; 
     
    public String getRollNo()  
    { 
        return rollNo; 
    } 
     
    public void setRollNo(String rollNo)  
    { 
        this.rollNo = rollNo; 
    } 
     
    public String getName()  
    { 
        return name; 
    } 
     
    public void setName(String name)  
    { 
        this.name = name; 
    } 
} 


// View
class StudentView  
{ 
    public void printStudentDetails(String studentName, String studentRollNo) 
    { 
        System.out.println("Student: "); 
        System.out.println("Name: " + studentName); 
        System.out.println("Roll No: " + studentRollNo); 
    } 
} 

// Controller
class StudentController  
{ 
    private Student model; 
    private StudentView view; 
  
    public StudentController(Student model, StudentView view) 
    { 
        this.model = model; 
        this.view = view; 
    } 
  
    public void setStudentName(String name) 
    { 
        model.setName(name);         
    } 
  
    public String getStudentName() 
    { 
        return model.getName();         
    } 
  
    public void setStudentRollNo(String rollNo) 
    { 
        model.setRollNo(rollNo);         
    } 
  
    public String getStudentRollNo() 
    { 
        return model.getRollNo();         
    } 
  
    public void updateView() 
    {                 
        view.printStudentDetails(model.getName(), model.getRollNo()); 
    }     
} 

// Driver class
class MVCPattern  
{ 
    public static void main(String[] args)  
    { 
        Student model  = retriveStudentFromDatabase(); 
  
        StudentView view = new StudentView(); 
  
        StudentController controller = new StudentController(model, view); 
  
        controller.updateView(); 
  
        controller.setStudentName("Vikram Sharma"); 
  
        controller.updateView(); 
    } 
  
    private static Student retriveStudentFromDatabase() 
    { 
        Student student = new Student(); 
        student.setName("Lokesh Sharma"); 
        student.setRollNo("15UCS157"); 
        return student; 
    } 
      
} 

When to use?
------------
- With UIs.
- Helps many devs work on each part.
- Supports different technologies.
- Can have many Views for one Model.