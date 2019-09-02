Java notes:
===========
Tips:
-----
- Anything between //// and //// is code.
- This note is not from a single source, it's compiled from tutorialspoint, w3schools, and other stuff.
------------------------------------------------------------------------------------------------------------------
Basics:
-------
First, basics:
- Java is a programming language AND a platform.
- The java programming language is a high-level OOP language, it is platform independent,  secure, portable, multithreaded, high-performance, distributed, dynamic, and its bytecode is interpreted.
- The java platform is the environment in which java code runs, it's basically a Java Virtual Machine (JVM) and an API specific to that platform.
- The JVM is the compiler of Java, sort of.

Mainly there are four platforms:
1. Java SE: the normal regular java, Java Standard Edition (SE):
    - API: has the core functionality of Java, like basic types, objects, classes, networking, security, DB access, GUI dev, and XML parsing.
    - JVM: it has a JVM, dev tools, deployment technologies, other libraries, etc.

2. Java EE: a platform built on top of Java SE.
    - API: had additional API for developing and running large-scale multi-layer reliable and secure network apps.
    - JVM: is designed to run such programs.

Now, let's delve into Java.
------------------------------------------------------------------------------------------------------------------
Java SE (Tutorialspoint):
-------------------------
A java SE program consists of Objects, Classes, Methods, Instance variables.

- Anything in Java must be contained in classes, the entry point of Java is the class that has the method 'main' in it.
- Because of that, the method must be public.
- Also, we don't have to make an instance of the class containing it to use it i.e. it has to be static.
- So, the main function, the entry point in Java, looks like this:
//// Java
public class IAmIronMan {
    public static void main(String args[]) {
        //write your code here
    }
}
//// ENDCODE

- The file containing the code should be named exactly after the public class name (If we don't have public classes in a file, its name can be whatever).

Java has:
- Access modifiers, you know public, default, protected, private.
- Non-access modifiers: final, abstract, strictfp. We'll look at these later.
- Variables, obviously.
- Arrays, obviously. An array is an object on the heap.
- Enums, also obviously.

Here's a simple java code:
//// Java
class Avenger {
    enum AvengerName { IronMan, Cap, Thor, Hulk, BlackWidow};
    AvengerName name;
    int powerLevel;
}

public class Assemble {
    public static void main(String args[]) {
        Avenger ironMan = new Avenger();
        ironMan.name = Avenger.AvengerName.IronMan;
        ironMan.powerLevel = 99;
        System.out.println("I am " + ironMan.name);
    }
}
//// ENDCODE

- Java supports Inheritance: creating classes (Child classes, subclasses) that extend other classes (Parent classes, superclasses).
- Java supports Interfaces: an interface is like a contract between objects on how to communicate, they are very important in inheritance.

Objects and Classes:
--------------------
You know what OOP is:
- Polymorphism, inheritance, encapsulation, abstraction, classes, objects, instances, methods, message passing.

So, we'll look at the syntax and any details:
- Check out chaining constructors.
- Which lets us set some constructors as private.

//// Java
public class LargeStep {
    //  <<<<<< Constructors >>>>>
    int members;
    String todaysTopic;

    public LargeStep(int noOfmembers) {
        members = noOfmembers;
    }

    public LargeStep(int noOfmembers, String topic) {
        //members = noOfmembers;
        //instead of repeating code
        //we can call constructors
        //from other constructors
        //like this:

        this(noOfmembers);

        //but this have to be the
        //first line in this
        //constructor

        todaysTopic = topic;
    }

    public void start3omq() {
        System.out.println("Enaharda hntkalm fe " + todaysTopic);
    }

    public static void main(String[] args) {
        //  <<<<<< Objects >>>>>
        // LargeStep muchLive: declaration of the variable.
        // new: making a new object of what follows.
        // LargeStep(..): this is a call to a constructor, this makes the new object.
        LargeStep muchLive = new LargeStep("3omq");

        System.out.println(muchLive.members);

        muchLive.start3omq();
    }
}

//  <<<<<< Declaration >>>>>


//// ENDCODE

Initializer blocks:
-------------------
IF we have some code we want to share across all constructors, we use initializer blocks:

//// Java
public class LargeStep {
    //  <<<<<< Constructors >>>>>
    int members;
    String todaysTopic;

    {   //here you go, init block
        //code that is shared across all constructors.
    }

    public LargeStep(int noOfmembers) {
        members = noOfmembers;
    }

    public LargeStep(int noOfmembers, String topic) {
        //members = noOfmembers;
        //instead of repeating code
        //we can call constructors
        //from other constructors
        //like this:

        this(noOfmembers);

        //but this have to be the
        //first line in this
        //constructor

        todaysTopic = topic;
    }

    public void start3omq() {
        System.out.println("Enaharda hntkalm fe " + todaysTopic);
    }

    public static void main(String[] args) {
        //  <<<<<< Objects >>>>>
        // LargeStep muchLive: declaration of the variable.
        // new: making a new object of what follows.
        // LargeStep(..): this is a call to a constructor, this makes the new object.
        LargeStep muchLive = new LargeStep("3omq");

        System.out.println(muchLive.members);

        muchLive.start3omq();
    }
}
//// ENDCODE

Order of initialization:
------------------------
0. Static init block (because it executes when the CLASS is loaded, even before we declare any instances).
1. Field init.
2. Init block.
3. Constructor.

Some notes:
-----------
- Java supports singleton classes.

Rules about .java source files:
- Only one public class per source file.
- As many non-public classes per file.
- The public class name should be the source file's name.java.
- If the class is defined in a package, the package should be the first statement in a file.
- imports are written after the package, before the class declaration.
- If there's no package, imports are the first thing.

Packages, imports, and FQN:
---------------------------
- Package: Basically a grouping of classes.
- FQN: Fully qualified name, a class name preceded by the package that contain it, which is preceded by the package that contain it, etc.
    NOTE: this applies to variables inside classes, etc.
- import: the same as include, it includes other packages and classes to use here.

Data types in Java: (Notes only)
-------------------
- Primitives: byte, short, int, long, float, double, boolean, char, String.
- References: they are created using defined constructors.
    - A reference variable can refer toany object of the declared type.

Variables: (Notes only)
----------
- Local variables are allocated on the stack.
- There is no default value for local variables, they should be initialized manually.

- Instance variables are allocated on the heap.
- There are default values for instance variables, numbers: 0, boolean: false, references: null.
- Instance variables can be called by their name only inside a class, but should be called by their FQN  in STATIC methods.

- Class variables are instance variables that are declared using 'static'.
- One copy only in the class, all objects share that copy.
- Static variables are stored in static memory.
- It's rare to use them except for declared finals or for constants.
- Created when the program starts, destroyed when it ends.
- Default values just like instance variables.
- They can be assigned special blocks to initialize them, called Static blocks.


Modifiers: (notes only)
----------
- Access modifiers: public, protected, private, default.
    - Protected: can be accessed by subclasses in other packages, or ANY classes within the package of the protected member's class.
    - Public and protected members are inherited, private members are not.

- Non-access modifiers:
    - Static: we saw that. Static methods don't use instance variables, they get all their data from arguments. 
    - final: a final ref variable cannot be reassigned, but the value can be changed.
        - But a primitive final can't be changed.
        - So 'static final int x = 10;' is equivalent to 'const int x = 10'; 
        - Final methods can't be overridden.
        - Final classes can't be subclassed.
    - abstract: for abstract classes and methods.
        - Abstract classes are never initialized, they are used only for subclassing.
        - So, a class can't be abstract and final.
        - If a class has abstract methods, it must be abstract.
        - An abstract class can contain normal methods and abstract methods.
        - An abstract method has no implementation because it's going to be overridden by a subclass.
        - Any class extending an abstract class must implement ALL abstract methods in it, unless the subclass is also abstract.
    - synchronized: a synchronized method can be accessed by one thread at a time.
    - transient: a transient instanve variable is skipped by the JVM when serializing the object containing it. 
    - volatile: any thread accessing this variable must merge its private copy of the variable with the master copy in the memory.

Arrays:
-------
Arrays are fixed-size.

//// Java
int[] a;
//or 
int b[];
//but the first is preferred.

//or
int[] c = {1, 2, 3 ...};

//or
int[] d = new int[10]; 

//this statement does two things:
    //1) creates the array.
    //2) assigns a reference to it, that reference is d.
//// ENDCODE

Arrays can be passed to methods and returned from methods:
//// Java
public static void printArray(int[] a) {
    for(int element: a) {
        System.out.println(e + ", ");
    }
}

public static int[] reverse(int[] arr) {
        int[] result = new int[arr.length];
        for(int i = 0 ; i < result.length ; i++) {
            result[result.length - i - 1] = arr[i];
        }
        return result;
    }
//// ENDCODE

Additionals:
------------
- Date & time classes & objects.
- Regexps:
    - Pattern class: a compiled representation of a regex.
    - Mather class: an engine that matches a pattern with an input string.
        - It can detect, replace, and do other cool stuff.
    - PatternSyntaxException: an exception that indicates a syntax error in the regex pattern.
 
Methods:
--------
- Basics are just like C++.
- Params can be passed by value or ref.
- Overloading is supported.
- public static void main(String args[]): the args[] are command-line args you pass.
- Variable number of args:
    //// Java
    public static double getMax(double... numbers) { //hey that looks like JS
        if(numbers.length == 0) {
            System.out.println("7a?");
            return;
        }

        double result = numbers[0];
        for(int i = 1 ; i < numbers.legnth ; i++) {
            if(numbers[i] > result)
                result = numbers[i];
        }
        
        return result;
    }
    //// ENDCODE

- finalize(): basically a destructor.
    //// Java 
    public clas IHaveBeenTerminated {
        protected void finalize() {
            tellMyWifeILoveHer();
        }
    }
    //// ENDCODE

this:
-----
Unlike fucking JS, this keyword simply enough references the object of the current class.

IO and files:
-------------
A package for reading and writing files and streams.
- InputStream: reads from source.
- OutputStream: writs to destination.

Some awesome functionality supported by Java:
- Byte streams: writes/reads 8-bit bytes, classes:
    - FileInputStream, FileOutputStream.
- Character streams: same but 16-bit, classes:
    - FileReader, FileWriter (both use FileInputStream and FileOutputStream internally, but they read/write two bytes at a time).
- Standard streams: this is the same as stdio.h, iostream.h, all that stuff.
    - Standard Input: System.in, class: InputStreamReader.
    - Standard Output: System.out
    - Standard Error: System.err

//// Java
import java.io.*;
public class LookAtThisPhotograph {
    public static void main(String args[]) throws IOException {
        InputStreamReader cin = null;
        try {
            cin = new InputStreamReader(System.in); //initializing it
            char c;
            do {
                c = (chat) cin.read(); //here's the input part
                //do stuff with c
            } while(c != 'q');
        } finally {
            if(cin != null) {
                cin.close(); //closing the input stream
            }
        }
    }
}
//// ENDCODE

You can also manage directories in Java e.g. creating, listing, etc.

Exception handling:
-------------------
There are three types of exceptions in java:
- Checked: an exception that is notified by the compiler at compile time i.e. the compiler will say that there's an exception here and there. (They inherit from any other exception other than RuntimeException)
- Unchecked: these are runtime exceptions e.g. bugs. (These inherit from RuntimeException)
- Errors: these are not exceptions, but problems that happen beyond your control e.g. stack overflow, etc. (They inherit from Error, they are treated as unchecked exceptions, i.e. we don't handle these)

Java has exceptions and errors as classes.
Exceptions propagate up the call stack until someone catches them and handles them.

Handling exceptions:
--------------------
try catch finally, we know that, each catch gets a Type of the exception i.e. its datatype.
But you can do this:
//// Java
catch(IOException | FileNotFoundException e) {
    logger.log(e);
    throw e;
}
//// ENDCODE

If a method has code that can raise an exception and DOESN'T handle it, it should throw it to the caller, that is indicated in its signature.
A method can throw many exceptions
//// Java
public void doThis(int a, int b) throws IOException, FileNotFoundException {
    if(a > b)
        throw new IOException();
    else
        throw new FileNotFoundException();
}
//// ENDCODE

try-with-resources: When we use streams, resources, IO, etc. we need to close them in the finally block.
Or use try-with-resources so that it closes automatically.
//// Java
try(FileReader fr = new FileReader(filepath)) {
    // stuff
} catch() {
    // catch
}
//// ENDCODE

Any class that uses try-with-resources should implement AutoCloseable interface and close() method.
Resources declared in the brackets of a try block are final.

User-defined exceptions: if you like to add errors to your code because you're a psycho, you can do that in Java by extending Exception.
    Make two constructors:
        - One that accepts a string.
        - Another that accepts a string and the original exception.
Also your new error class must be a child of Throwable, which will happen when you extend Exception:
//// Java
class MyError extends Exception {
    public MyError() {
        System.err.println("Am I evil?");
        System.err.println("Yes I am!");
    }
}
//// ENDCODE

Nested classes:
---------------
Classes declared inside classes.
Types:
- Static: 
- Inner classes: these are a security mechanism in java, inner classes can be made private and access other private members of the outer class.
    Types of inner classes:
    - Inner:
    //// Java
    class JohnCena {
        private String finisher = "Attitude Adjustment";

        private class DoctorOfThuganomics {
            public void STF() {
                System.out.println("Michael Cole: Oooooooh myyyyyyy!");
            }

            // inner classes can access private members of outer classes ;)
            public void AA() {
                System.out.println("Michael Cole: And an " + finisher + " to end this match!");
            }
        }

        void MyTimeIsNow() {
            DoctorOfThuganomics johnCena = new DoctorOfThuganomics();
            johnCena.STF();
        }
    }

    public class Main {
        public static void main(String args[]) {
            JohnCena jc = new JohnCena();
            jc.MyTimeIsNow();
        }
    }
    //// ENDCODE

    - Method-local: classes declared WITHIN a method, their scope is just like any local variable.
    //// Java
    class JohnCena {
        private String finisher = "Attitude Adjustment";

        private class DoctorOfThuganomics {
            public void STF() {
                System.out.println("Michael Cole: Oooooooh myyyyyyy!");

                class TapOut {
                    public void printThat() {
                        System.out.println("Michael Cole: He tapped out! He tapped out!");
                    }
                }

                TapOut t = new TapOut();
                t.printThat();
            }

            // inner classes can access private members of outer classes ;)
            public void AA() {
                System.out.println("Michael Cole: And an " + finisher + " to end this match!");
            }
        }

        void MyTimeIsNow() {
            DoctorOfThuganomics johnCena = new DoctorOfThuganomics();
            johnCena.STF();
        }
    }

    public class Main {
        public static void main(String args[]) {
            JohnCena jc = new JohnCena();
            jc.MyTimeIsNow();
        }
    }
    //// ENDCODE

    - Anonymous: inner class declared without a name, we declare and instantiate them at the same time, used to override methods of a class or interface, very similar to IIFEs of JS:
    //// Java
    class JohnCena {
        private String finisher = "Attitude Adjustment";

        private class DoctorOfThuganomics {
            public void STF() {
                System.out.println("Michael Cole: Oooooooh myyyyyyy!");

                class TapOut {
                    public void printThat() {
                        System.out.println("Michael Cole: He tapped out! He tapped out!");
                    }
                }

                TapOut t = new TapOut();
                t.printThat();
            }

            // inner classes can access private members of outer classes ;)
            public void AA() {
                System.out.println("Michael Cole: And an " + finisher + " to end this match!");
            }
        }

        void MyTimeIsNow() {
            DoctorOfThuganomics johnCena = new DoctorOfThuganomics();
            johnCena.STF();
        }
    }

    abstract class TheMiz {
        public abstract void finisher();
    }

    public class Main {
        public static void main(String args[]) {
            JohnCena jc = new JohnCena();
            jc.MyTimeIsNow();
            TheMiz miz = new TheMiz() {
                public void finisher() {
                    System.out.println("Michael Cole: SKULL CRUSHING FINALE!!");
                }
            };

            miz.finisher();
        }
    }
    //// ENDCODE

If a method takes an object of an interface, abstract class, concrete class, then we can implement the interface, extend the abstreact class, and pass the object to the method i.e. pass an anonymous inner class as argument to a method that accepts instance of interface or abstract or concrete.
//// Java
    class JohnCena {
        private String name = "John..Ceeenaaaaa";
        private String finisher = "Attitude Adjustment";

        public String getName() {
            return name;
        }

        private class DoctorOfThuganomics {
            public void STF() {
                System.out.println("Michael Cole: Oooooooh myyyyyyy!");

                class TapOut {
                    public void printThat() {
                        System.out.println("Michael Cole: He tapped out! He tapped out!");
                    }
                }

                TapOut t = new TapOut();
                t.printThat();
            }

            // inner classes can access private members of outer classes ;)
            public void AA() {
                System.out.println("Michael Cole: And an " + finisher + " to end this match!");
            }
        }

        void MyTimeIsNow() {
            DoctorOfThuganomics johnCena = new DoctorOfThuganomics();
            johnCena.STF();
        }
    }

    abstract class TheMiz {
        public abstract void finisher(JohnCena jc);
    }

    public class Main {
        public static void main(String args[]) {
            JohnCena jc = new JohnCena();
            jc.MyTimeIsNow();
            TheMiz miz = new TheMiz() {
                public void finisher(JohnCena jc) {
                    System.out.println("Michael Cole: SKULL CRUSHING FINALE ON " + jc.getName() + "!!");
                }
            };

            miz.finisher(new JohnCena() {
                private String name = "The champ";
                public String getName() {
                    return name;
                }
            });
        }
    }
//// ENDCODE

Static nested classes:
an inner static class, like any static member:
    - can be used without instance of outer.
    - can access other statics.
    - can't access non-statics.

------------------------------------------------------------------------------------------------------------------
Java SE OOP Concepts:
----------------------
Inheritance:
------------
//// Java
public class Main {
    class Wrestler {
        private String finisher;

        public Wrestler(finisher) {
            this.finisher = finisher;
        }

        public void applyFinisher() {
            System.out.println(finisher);
        }
    }

    class Superstar extends Wrestler {
        private String signatureMove; 

        public Superstar(finisher, signatureMove) {
            this.finisher = finisher;
            this.signatureMove = signatureMove;
        }

        public void goForWin() {
            System.out.println(signatureMove);
            System.out.println(finisher);
        }
    }

    public static void main(String args[]) {
        Superstar johnCena = new Superstar("Five Knuckle Shuffle", "Attitude Adjustment");
        johnCena.applyFinisher();
        johnCena.goForWin(); 
    } 
}
//// ENDCODE

A superclass variable can refer to a subclass object but can only access superclass members.

super keyword: like any super keyword, it invokes stuff of superclass from inside subclass.

If there are two variables with the same name in the super and the sub, to use the super's we use super.varName.

Subclasses automatically call default constructor of superclass, if you want to call other constructors use 'super(arguments);'

Java supports:
- single inheritance.
- multilevel inheritance.
- hierarchial inheritance.

Java doesn't support multiple inheritance i.e. a subclass inheriting from two or more superclasses.

But a class can implement many interfaces.

You can know the type of a varibale using 'instanceof'.

implements: When a class inherits from an interface, not a class, it 'implements' it, so we use the keyword 'implements' instead of 'extends'.

Polymorphism:
-------------
Java supports function overriding in inheritance.

But, if we create a subclass and refer to it with a superclass reference, we can only use overridden methods by that subclass:
//// Java
    class Samurai {
        public void strike() {
            System.out.println("sword strike");
        }
    }

    class Afro extends Samurai {
		@Override //You have to precede an overriding method with this annotation here
        public void strike() {
            System.out.println("hit with sandals");
        }
        public void playDirty() {
            System.out.println("throw cigarette into eye");
        }
    }

public class Main {
    public static void main(String args[]) {
        Samurai afrosDad = new Afro();
        Afro afro = new Afro();

        afrosDad.strike();
        afro.strike();

        afro.playDirty();
        //afrosDad.playDirty will give an error
    } 
}
//// ENDCODE

Rules of overriding:
- same args.
- same return type.
- can't have more restrictive access level than the overridden.
- final can't be overridden.
- static can't be overridden but can be re-declared.
- subclass in same package as superclass of instance can override any non-private-or-final superclass method.
- subclass in DIFFERENT package overrides non-final public/protected methods.
- overriding methods can throw unchecked exceptions, but not checked exceptions that are not thrown by the overridden.
- constructors are not overridden.

To invoke the overridden method from the subclass, use 'super.method()'.

Using this in polymorphism:
---------------------------
We can use superclass references to refer to subclass instances.
//// Java
public interface Champ {}
public class JohnCena {}
public class DoctorOfThuganomics extends JohnCena implements Champ {}

Champ champ = new Champ();
DoctorOfThuganomics doctor = champ;
JohnCena jc = champ;
Object o = champ;
//// ENDCODE

All of them refer to the same Champ object in the heap, but each can access different methods and stuff.

Virtual methods: any reference to an object that overrides its parent in a particular method, will invoke the overridden method even if the reference used is a superclass reference.

Abstraction:
------------
Use abstract keyword to declare methods and classes abstract so that they can't be instantiated or have to be overridden.

Encapsulation:
--------------
Using access modifiers, then providing setters and getters.

Interfaces:
-----------
It's like a class but only unimplemented methods i.e. behaviors of an object.
They are like contracts, classes implement them i.e. classes conform to the contract defined by the interfaces.

For example, if we want a class to be put into an array and for that array to be able to be sorted by Arrays.sort, we must make that class implement Comparable interface, and compareTo method.
//// Java
class Pokemon implements Comparable {
    private int powerlevel = 0;

    public Pokemon(int powerlevel) {
        this.powerlevel = powerlevel;
    }

    public int compareTo(Object o) {
        Pokemon p = (Pokemon) o;
        return this.powerlevel - p.powerlevel;
    }
}

public class Main {
    public static void main(String args[]) {
        Pokemon pikachu = new Pokemon(100);
        Pokemon totodile = new Pokemon(90);
        Pokemon chikorita = new Pokemon(80);
        Pokemon mewtwo = new Pokemon(999);
        Pokemon blastoise = new Pokemon(800);
        Pokemon entei = new Pokemon(600);

        Pokemon[] myPokemonSet = {pokachu, totodile, chikorita, mewtwo, blastoise, entei};
        Arrays.sort(myPokemonSet); //We can do that :o
    }
}
//// ENDCODE
That's how we take advantage of the efficient Arrays.sort.
Same for classes we want forEach to operate on them, we need to implement Iterable.
(But usually, we implement a separate class for iterator, for example:
////Java
class PokemonIterator implements Iterator<Pokemon> {
    private Pokemon[] pokemons;
    private int index = 0;

    boolean hasNext() {
        return index < pokemons.length;
    }

    public Pokemon next() {
        Pokemon p = pokemons[index];
        index++;
        return p;
    }
}
//// ENDCODE
)
Of course, you can implement both interfaces to give the ability for the class to be sorted and iterated over.
This is the power of implementing multiple interfaces and for interfaces to extend multiple other interfaces.
This is how multiple inheritance is achieved in Java without classes extending multiple classes.

So, interfaces are like sockets for external plugins, any plugin should have the same socket type e.g. USB, VEGA, HDMI, etc.


Props:
- They are a reference type.
- They can contain methods, abstract methods, constants, default, static, and nested types.
- Only static and default methods get to have a body.
- interfaces can be written in separate files just like classes.
- interfaces appear in packages.
- We cannot make an instance of an interface, they are implicitly abstract.
- Methods in an interface are abstract and public.
- An interface can extend many interfaces, and is implemented, not extended, by a class.

Generic Interfaces:
- Same concept as generics.
//// Java
public interface Comparable<T> {
    int compareTo(T o);
}
//// ENDCODE
In this case, we don't have to stick to a certain type when overriding the method, we don't have to explicitly cast the parameter.

Tagging interfaces: interfaces with no body.
Why tagging interfaces?
- Create a common parent for any implementing class.
- Add data type to class, the class doesn't need to define any methods of an empty interface, but it becomes an interface type through polymorphism.

Packages:
---------
We can define our own packages.

To create a package:
- choose a name
- include the statement 'package packageName;' on top of every source file that has the stuff you want in that package.
- Then, compile:
>> javac -d DestinationFolder filename.java
    The folder created will have another folder inside it with the package name, where the compiled files are.

To include a package in your code:
'import packageName;'
- If we want to import a certain class from the package, do this:
'import packageName.className;' //a FQN of the class

Directory structure of packages:
--------------------------------
When a class is placed in a package, two things happen:
- the package name becomes part of the FQN of the class.
- the package name must match the directory structure where the bytecode resides.

To manage your files in Java:
- put source code of class, interface, enums, etc. in a text file.
e.g. file.java
- Now put that file in a directory with the package name you want the source file to belong to.
e.g. classes/com/itworx/awesomePackage/file.java
- Now compile:
>> javac -d . Dell.java
The files will be compiled in a destination (which can be different from where the source code is) folder with the name of the package.
e.g.:
    sources/com/itworx/awesomePackage/file.java
    classes/com/itworx/awesomePackage/... .class
- You can now import the package from the directory.
    import com.itworx.awesomePackage.*;

This helps by:
- giving access to classes to programmers without revealing your sources.
- helps the JVM and compiler to find all types your program uses.
- The path: classes/com/itworx ... etc. is called the class path.
- This is set by CLASSPATH environment variable.
This is where JVM will look for classes, interfaces, etc.
- A class path may have many pathes in it.
- By default the JVM searches current directory and JAR file containing the java platform classes, they are in the class path by default.

------------------------------------------------------------------------------------------------------------------
Advanced Java SE:
-----------------
Data structures in Java:
------------------------
There are several DS in java that help us do things e.g.
enum, bitset, vector, stack, dictionary, hashtable, properties, etc.

Collections:
------------
It's a framework that has a lot of interfaces of DS's, the standard implementations of these interfaces work in a similar manner and are interoperable.
Also it can be extended easily e.g. we can add collections.

A collection framework:
    A unified architecture for representing and manipulating collections, it can has:
        - interfaces.
        - Concrete classes that implement the collection interfaces.
        - Algorithms that are useful for data structures, they are polymorphic i.e. the same algorithm can be used on many different implementations of the same collection interface.

Interfaces of collection frameworks:
------------------------------------
- Collections:
- List:
- Set:
- SortedSet:
- Map:
- Map.Entry:
- SortedMap: 
- Enumeration:

Classes of collection frameworks:
---------------------------------
- AbstractCollection: implements Collection.
    - AbstractList: implements List.
        - ArrayList: implements a dynamic array.

    - AbstractSequentialList: uses sequential access, not random access.
        - LinkedList: implements a LL.

    - AbstractSet: implements Set.
        - HashSet: for use with a hash table.
            - LinkedHashSet.

        - TreeSet: implements a tree.

    - AbstractMap: implements Map. 
        - HashMap: a hash table.
        - TreeMap: tree.
        - WeakHashMap: hash table with weak keys.
        - LinkedHashMap.
        - IdentityHashMap: uses reference equality when comparing documents.

Legacy Classes:
---------------
Vector, Stack, Dictionary, Hashtable, Properties, BitSet.

Collections has also Algorithms to help.

Generics:
---------
- Methods and classes that accept any type of data and apply the same algorithm on them.

Generic Methods:
//// Java
public static <E> void printArray(E[] arr) {
        for(E elem: arr) {
            System.out.print(elem + " ");
        }
        System.out.println();
    }
//// ENDOCDE

Bounded type parameters:
- Restrict the kinds of data types allowed in some generics e.g. allowing only Numbers e.g. integer, float, etc. but not Boolean.

//// Java
public static <T extends Comparable<T>> T getMax(T... x) {
    if(x.length == 0) {
        return;
    }

    T result = x[0];
    for(int i = 0 ; i < x.length ; i++) {
        if(x[i].compareTo(result) > 0) {
            result = x[i];
        }
    }

    return result;
}
//// ENDCODE

Generic Classes:
----------------
These are used for example in collections that are generic e.g. ArrayList, HashMap, etc.
//// Java
class Box<T> {
    private T t;

    public void doThis(T x) {
        //do something with t and x
    }

    //etc.
}
//// ENDCODE

Serialization:
--------------
it's a mechanism where an object can be represented as a sequence of bytes that include the object's data as well as info about its type and the type of data in it.

This can be used to serialize then write, or read then deserialize, to and from files.

This process is JVM independent i.e. an object can be serialized and written on one platform, then read and deserialized on another.

Classes to do this:
- ObjectInputStream: the method readObject() reads from a stream then deserializes it.
- ObjectOutputStream: the method writeObject(Object x) serializes x and sends it to an output stream.

To enable a class to be serialized:
- It must implement java.io.Serializable.
- All fields in it must be serializable (i.e. they don't have the keyword transient).

How to serialize:
- Open a try block:
    - Create a FileOutputStream.
    - Create an ObjectOutputStream and pass the FileOutputStream into the constructor.
    - ObjectOutputStream.wrietObject(give your object here).
    - Close the file stream.
- Catch an IOException in a catch block.

How to deserialize:
- try
    - FileInputStream
    - ObjectInputStream, pass FileInputStream into constructor.
    - e = (datatype for casting) ObjectInputStream.readObject();
    - cloase the file stream.
- catch an IOException and a ClassNotFoundException.

Why ClassNotFoundException?
- JVM must be able to find the bytecode of the class you want to deserialize, if not, this exception is thrown.

transient fields won't be send to the output stream.

Networking:
-----------
java.net package baby!
It contains classes and stuff for low-level communication like TCP, UDP, etc.

Sockets:
--------
Classes:
- java.net.Socket: for the client.
- java.net.ServerSocket: for the server.
- InetAddress: a class representing IP address, which has some useful methods.

How to make a TCP connection between two computers:
- Server creates ServerSocket object, with a port.
- Server invokes ServerSocket.accept(), this method waits for a client to connect.
- Client creates Socket object, specifying server name and port.
- Now Socket object will try to connect (in the constructor).
- Server, if success, accept() returns a reference to new socket on the Server.

Now both can send/receive through IO streams.
Each socket has OutputStream and InputStream.

Emails:
-------
To send emails using Java, you need:
- JavaMail API
- Java Activation Framework (JAF)
installed on your machine.

Multithreading:
---------------
Java supports multithreadid programming.

Life cycle of a thread:
- Creation: New Thread();
- Thread starts: Start();
- Thread is running: run();
- Thread waits for specific event: Sleep(), wait();
- Thread finishes execution and dies.

Threads can be given priorities in Java.
Higher priority gets time allocated before time of other threads gets allocated.

How to use threads (Method 1):
- To make a class get executed as a thread you should implement the Runnable interface:
    - Step 1: implement public void run() method, here put your business logic.
    - Step 2: instantiate a Thread object:
        Thread(Runnable threadObj, String threadName);
    - Step 3: Now you can call start() which executes run().

Example:
//// Java
class FirstThread implements Runnable {
    private Thread t;
    private String name;

    public FirstThread(name) {
        this.name = name;
    }

    public void run() {
        //put the code you want to run here
    }

    public void start() {
        if(t == null) {
            t = new Thread(this, name);
            t.start();
        }
    }
}
//// ENDCODE

How to use threads (Method 2):
- Extend the Thread class:
    - Step 1: override run() method.
    - Step 2: call start();

Example:
//// Java
class NewThread extends Thread {
    private Thread t;
    private String name;

    NewThread(String name) {
        this.name = name;
    }

    public void run() {
        //business logic here
    }

    public void start() {
        //same code as above
    }
}
//// ENDCODE

Thread class has normal methods and static methods.
Invoking a static method makes it perform on the currently running Thread.

Applets:
--------
Applets are Java programs that run on a browser.
- Applet is a Java class that extends java.applet.Applet.
- Applets don't use main() to start.
- Applets are embedded inside HTML.
- When the user displays a webpage with an applet, it's downloaded to the user's machine.
- A JVM is required to run the applet, it can be a plugin of the web browser or installed on the user's machine.
- The JVM creates an instance of the Applet's class and invokes many methods during the Applet's lifetime.
- If the applet needs other Java classes it can get them in a single JAR file.

Lifecycle of an applet:
- init: instantiation.
- start: starting the applet, called when the page is loaded or when the user returns to the page.
- paint: invoked right after start.
- stop: when the user moves off the page.
- destroy: when the browser is shut down.

//// Java
import java.applet.*;
import java.awt.*; //to get Graphics object

public class HelloWorldApplet extends Applet {
    public void paint(Graphics g) {
        g.drawString("Hello World", 25, 50);
    }
}
//// ENDCODE

Applet class has methods to get applet parameters, network location of the HTML file or applet class directory, print status messages, fetch images and audio, play audio, resize the applet, among other stuff.

Also, it has an interface for the user so that they can request info, initialize, start, stop, and destroy the applet.

Embedding in HTML:
//// HTML Java
<html>
   <title>The Hello, World Applet</title>
   <hr>
   <applet code = "HelloWorldApplet.class" width = "320" height = "120">
      If your browser was Java-enabled, a "Hello, World"
      message would appear here.
   </applet>
   <hr>
</html>
//// ENDCODE

Capabilities:
- You can get and specify params of applets.
- You can convert a graphical Java app to an applet.
- Applets inherit event-handling methods from Container class e.g. processMouseEvent, processKeyEvent (stuff like React, JS, etc.)
- You can display images, play audio, etc.

Documentation:
--------------
Javadoc: a tool in JDK to generate documentation in HTML format.
It's written in its own tags.
We can specify HTML tags.
Example:
//// Java
/**
* <h1>Hello, World!</h1>
* The HelloWorld program implements an application that
* simply displays "Hello World!" to the standard output.
* <p>
* Giving proper comments in your program makes it more
* user friendly and it is assumed as a high quality code.
* 
*
* @author  Zara Ali
* @version 1.0
* @since   2014-03-31 
*/
public c

//source code here

//// ENDCODE

Done with Java SE ^_^
------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------
Java 8:
-------
Lambda expressions:
-------------------
parameter -> expression body;

Props:
- No need to declare the type of param.
- Parentheses for multi params.
- Curly braces for multi-line expression.
- Use 'return' if curly braces.

They are very similar to arrow functions in JS.
Example:
//// Java
public class Main {
    interface MathOperation {
      int operation(int a, int b);
    }

    private static int operate(int a, int b, MathOperation m) {
        return m.operation(a, b);
    }

    public static void main(String args[]) {
        MathOperation add = (a, b) -> a + b;
        MathOperation sub = (a, b) -> a - b;
        MathOperation mul = (a, b) -> a * b;
        MathOperation div = (a, b) -> a / b;

        System.out.println(operate(3, 2, add));
        System.out.println(operate(3, 2, sub));
        System.out.println(operate(3, 2, mul));
        System.out.println(operate(3, 2, div));
    } 
}
//// ENDCODE

- Lambda expressions are usually used for implementing functional interfaces i.e. interfaces with one method.
- They eliminate the need of anonymous classes.

Scope of lambda expressions:
- Lambda expressions can refer to any final variable.

Method References:
------------------
- Referring methods by name, this can be used for all methods: static, instance methods, or even constructors.
//// Java
import java.util.*;

public class Main {
    public static void main(String args[]) {
        List num = new ArrayList();
        num.add(1);
        num.add(1);
        num.add(1);
        num.add(1);
        num.add(1);

        num.forEach(System.out::println);
    } 
}
//// ENDCODE


Iterator:
---------
//// Java
ArrayList list = new ArrayList();
Iterator it = list.iterator();
while(it.hasNext()) {
	Object element = it.next();
	//do stuff with that
}
//// ENDCODE

Static initializer:
-------------------
A block to initialize static stuff.
Executed when the class is loaded.
You can use them to create instances, and exception handling during initialization of a class.
You can use them to read from files the necessary data for processing of any instance.
//// Java
static Scanner input = new Scanner(System.in);
static boolean flag = true;
static int B = input.nextInt();
static int H = input.nextInt();

static{
    try{
        if(B <= 0 || H <= 0){
            flag = false;
            throw new Exception("Breadth and height must be positive");
        }
    }catch(Exception e){
        System.out.println(e);
    }
}

//// ENDCODE

Is java pass by value or by reference?
--------------------------------------
- Primitive types: by value.
- Things that are constructed using the 'new' keyword: by reference, because the variables are actually references:
//// Java
class Pokemon {
    private String name;

    public Pokemon() {
        this.name = "unown";
    }

    public Pokemon(String name) {
        this.name = name;
    }
}
public class Main {
    public static void main(String args[]) {
        Pokemon p1 = new Pokemon('pikachu');
        Pokemon p2 = new Pokemon('totodile');
    }
}
//// ENDCODE

p1 and p2 don't hold the values of class Pokemon.
They hold REFERENCES to the created objects.
So, when we pass p1 and p2 to a function.
They are passed by value, but their value is REFERENCE to objects.
So the created objects are, in a sense, passed by REFERENCE.
So any change happening to MEMBERS of class instances, persist.

Variable number of parameters in a method:
------------------------------------------
public String doThis(int arg1, int arg2, int... arg3) {
    //code
}

The variable argument must be the last one.

Object class:
-------------
This is the root of all classes in Java.
So, an object class reference can point to ANYTHING.

Equality between class instances:
---------------------------------
Pokemon p1 = new Pokemon('totodile');
Pokemon p2 = new Pokemon('totodile');

- Now, p1 is NOT equal to p2, because these two REFERENCES point to different objects.
    f1 == f2 => false.
    f1.equals(f2) => false. //equals is a method from Object class.
- But we can override the method 'equals'
//// Java
class Pokemon {
    ...

    @Override
    public boolean equals(Object o) {
        Pokemon other = (Pokemon) o;
        return (this.name == o.name);
    }
}
//// ENDCODE

StringBuilder:
--------------
Strings are immutable, any modification in a string creates a new string, which is inefficient.
Stringbuilder probides a mutable string buffer.
    For best performance, the size of the buffer may be predefined. (It will grow if we exceed it, but that's inefficient)

Example:
//// Java
String location = "Nasr City";
String method = "plane";
int pos = 6;

StringBuilder sb = new StringBuilder(40);
sb.append("I flew to "); //it's added at the end of sb
sb.append(location);
sb.insert(pos, " by " + method + " ");

System.out.println(sb.capacity()); //the total capacity allocated for the string 
System.out.println(sb.length()); //length of the current string

String message = sb.toString();
System.out.println(message); //I flow by plane to Nasr City"
//// ENDCODE

Primitive Wrapper Classes:
--------------------------
Why?
- Classes provide methods and fields specific to type.
- As well as common interactions through Object class.

- But primitives are lightweight.

Primitive wrapper classes are classes that hold primitive values.
Examples:
- Boolean.
- Character.
- Integer.
- Number, which is abstract and has children:
    - Byte, Short, Integer, Long.
    - Float, Double.

All wrapper class instances are immutable, if you try to change the value inside a primitive wrapper, you'll get a reference to a new object.

Conversion from one to another:
- Primitive to Wrapper: 
    - some are done automatically.
        Integer a = 100; //a = references object Integer with value 100
        int b = a;  //b = 100;
        Integer c = b; //c = reference to Integer that has value of b.

    - some others are done by methods:
        - valueOf(): this is called Boxing, taking a primitive value and wrapping it up.
            Integer d = Integer.valueof(b);
        - valueOf(): from String to wrapper.

- Wrapper to Primitive: 
    - methods:
        - xxxValue(): xxx is the name of the primitive value e.g. doubleValue, intValue, etc.
            int e = d.intValue();
        - parseXxx(): from wrapper to String. (Not sure)

Equality of  wrapper classes:
-----------------------------
Integer a = 1000;
Integer b = 1000;
- a == b => FALSE, because they are references :o
- But a.equals(b): true, that works fine ^_^

But, a == b can evaluate to TRUE if:
    - int,
    - short,
    - byte,
    from -128 to 127.

Because Boxing in java always return the same wrapper class instance if it has the same value as a previously assigned class instance if it was one of these values.

Type imports:
-------------
Imports are NOT loading of things, they are just mapping for the compiler as to where to find some stuff.
- Single type import:
    import com.pluralsight.travel.Passenger; //this is importing a type

- Import on demand:
    import com.pluralsight.travel.*; //importing ALL the types from this package.

So why use single type imports anyway?
- With time, packages update, and increase in number of types, there's a great chance they'll use the same name for different types.
- So now when I compile my app, I get errors because some packages have types of the same name, with imports on demand they will collide.
- So, single type imports are actually preferred, and some IDEs provide automatic imports to make it easier.

Package access modifiers:
-------------------------
Some of the things that are provided by the package, are NOT meant to be used standalone, or outside the package.
So, packages provide some access boundary for some things.

Static:
-------
Static methods can only access static fields.

Static import:
//// Java
import static com.pluralsight.travel.className.staticMemberName;

//now we can directly use the static member either value or method.
//// ENDCODE
