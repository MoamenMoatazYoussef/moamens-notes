# Java Core

## Table of Contents
- [Java Core](#java-core)
  * [Table of Contents](#table-of-contents)
  * [Streams](#streams)
    + [Reading](#reading)
      - [Reader Class](#reader-class)
      - [InputStream Class](#inputstream-class)
    + [Writing](#writing)
      - [OutputStream](#outputstream)
    + [Error handling in streams](#error-handling-in-streams)
    + [Chaining Streams](#chaining-streams)
    + [File Streams & Buffered Streams](#file-streams---buffered-streams)
      - [Java.nio Classes For File Streams](#javanio-classes-for-file-streams)
      - [Path and Paths classes](#path-and-paths-classes)
      - [Specialized File Systems](#specialized-file-systems)
  * [String Formatting and RegExes](#string-formatting-and-regexes)
    + [String Formatting and Joinint](#string-formatting-and-joinint)
      - [StringJoiner](#stringjoiner)
      - [Format Specifiers](#format-specifiers)
      - [Writing Formatted Content To Streams](#writing-formatted-content-to-streams)
    + [Regular Expressions in Java](#regular-expressions-in-java)
  * [Collections](#collections)
    + [Collection Interface](#collection-interface)
    + [Collection features added in Java 8](#collection-features-added-in-java-8)
    + [Converting between Collections and Arrays](#converting-between-collections-and-arrays)
    + [Common Collection Types in Java](#common-collection-types-in-java)
      - [Common collection interfaces:](#common-collection-interfaces-)
      - [Common collection classes:](#common-collection-classes-)
      - [How these Collections are Sorted](#how-these-collections-are-sorted)
    + [Map Collections](#map-collections)
  * [Controlling App Execution and Environment](#controlling-app-execution-and-environment)
    + [Command-Line Arguments](#command-line-arguments)
    + [Dealing with Persistent Key/Value Pairs](#dealing-with-persistent-key-value-pairs)
      - [So how do you make this Properties object persistent?](#so-how-do-you-make-this-properties-object-persistent-)
        * [Simple text](#simple-text)
        * [XML](#xml)
      - [Default Values](#default-values)
  * [Class Loading](#class-loading)
    + [Execution Environment Information](#execution-environment-information)
  * [Capturing App Activity with the Log System](#capturing-app-activity-with-the-log-system)
    + [Logging Levels](#logging-levels)
    + [Log System in More Details](#log-system-in-more-details)
    + [Built-in Handlers](#built-in-handlers)
    + [Built-in Formatters](#built-in-formatters)
  * [Multithreading In Java](#multithreading-in-java)
    + [How to Make our Java Programs Multithreaded](#how-to-make-our-java-programs-multithreaded)
    + [Threading In Java](#threading-in-java)
    + [Thread Management Details in Java](#thread-management-details-in-java)
    + [Creating Closer Relationship Between Thread Tasks](#creating-closer-relationship-between-thread-tasks)
    + [Concurrency](#concurrency)
      - [What's happening?](#what-s-happening-)
    + [How to Solve Race Condition](#how-to-solve-race-condition)
    + [How to Protect Code That Runs On Multiple Method Calls (Manual Synchronization)](#how-to-protect-code-that-runs-on-multiple-method-calls--manual-synchronization-)
      - [Synchronized Methods](#synchronized-methods)
      - [Manual Synchronization Example](#manual-synchronization-example)
    + [Other types used for Concurrency](#other-types-used-for-concurrency)
  * [Type info and Reflections](#type-info-and-reflections)
    + [Types as Types](#types-as-types)
    + [How to access a type's _Class_ instance](#how-to-access-a-type-s--class--instance)
    + [How to access a Type's info](#how-to-access-a-type-s-info)
    + [How to access a Type's members](#how-to-access-a-type-s-members)
    + [Interacting with Object Instances](#interacting-with-object-instances)
    + [Creating instances of objects with Reflection](#creating-instances-of-objects-with-reflection)
      - [Another way to create instances in reflection](#another-way-to-create-instances-in-reflection)
  * [Adding Type Metadata with Annotations](#adding-type-metadata-with-annotations)
    + [Annotation](#annotation)
    + [Custom Annotations](#custom-annotations)
    + [A Closer Look At Elements](#a-closer-look-at-elements)
  * [Serialization](#serialization)
    + [Being Serializable](#being-serializable)
    + [How changes to class definition affects serialization](#how-changes-to-class-definition-affects-serialization)
    + [Custom Serialization](#custom-serialization)
    + [Transient Fields](#transient-fields)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Streams

### Reading

They are an ordered sequence of data, they are common io modules.
They are unidirectional i.e. we READ from, or we WRITE to, a single a stream.
Types:

- Byte streams: these are binary data.
- Text streams: these are unicode characters.

#### Reader Class

This is the class we use to read bytes

- int read(): returns the value of bytethat was read, written as int.
- int read(byte[] buff): returns the number of bytes that were read in the buffer.

#### InputStream Class

This is the class we use to read characters.

- int read(): returns the value of character that was read, written as int.
- int read(char[] buff): returns the number of characters that were read in the buffer.

Method we use with both classes:

How to read one byte:

```Java
InputStream input = // create input stream.
int intVal;
while((intVal = input.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    byte byteVal = (byte) intVal;
    //do something with byteVal
}
```

How to read one character:

```Java
Reader reader = // create input stream.
int intVal;
while((intVal = reader.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    char charVal = (char) intVal;
    //do something with charVal
}
```

How to read an array of bytes:

```Java
InputStream input = // create input stream
int length;
byte[] byteBuff = new byte[10];
while((length = input.read(byteBuff)) >= 0) { //notice that read(byte[] buff) returns the LENGTH of the bytes in the buffer, not the actual byte like the normal read()
    for(int i = 0 ; i < length ; i++) { //we can't assume the buffer is full, so we loop to the length only
        byte byteVal = (byte) byteBuff[i];
    }
}
```

Same thing for characters.

### Writing

#### OutputStream

This is the class we use to write bytes.

- void write(int b): writes the byte 'b' to a source.
- void write(byte[] buff): writes list of bytes to a source.

###@ Writer
This is the class we use to write characters.

- void write(char ch): writes the byte 'c' to a source.
- void write(char[] buff): writes list of characters to a source.
- void write(String str): writes string to a source.

How to write bytes:

```Java
OutputStream output = //create output stream
byte byteVal = 100;
output.write(byteVal);

//for many bytes
byte[] buff = {0, 63, 127};
output.write(buff)l
```

How to write characters or a string:

```Java
Writer writer = //create writer
char c = 'a';
writer.write(c);

//many character
char[] cbuff = {'a', 'w', 's'};
writer.write(cbuff);

//strings
String s = "Slim Shady";
writer.write(s);
```

### Error handling in streams

Errors happen, and streams throw exceptions.
Also, we need to clean up streams, we can't rely on the garbage collector of Java because streams usually depend on physical storage i.e. files, network connections, etc.
So, we need to provide some kind of reliable cleanup.

Streams implement Closeable interface, which as one method: close.
Opening the stream itself may throw errors, so we need to check if that happened or not before we close it.
Also, 'close' itself throws exceptions, so we need to put it in another try catch block.

```Java
Reader reader;
try {
    reader = Helper.openReader("filename.txt"); //this is what we do to open a reader for a certain file or source
    // do stuff with reader
} catch(IOException e) {
    System.out.println(e.getClass().getSimpleName() + "-" + e.getMessage());
} finally {
    if(reader != null) {
        try {
            reader.close();
        } catch(IOException e2) {
            System.out.println(e2.getClass().getSimpleName() + "-" + e2.getMessage());
        }
    }
}
```

Instead of doing this with every stream, we can use another interface: AutoCloseable.

```Java
interface AutoCloseable {
    void close() throws Exception;
}

interface Closeable extends AutoCloseable {
    void close() throws IOException;
}
```

So, a type implementing the AutoCloseable interface, supports a stable structure in Java called Try-with-resources.

It's a way to automate the process of closing a stream or any resource.
It handles the close method.
A resource is any type that implements AutoCloseable.

The syntax of try-with-resources statement:

```Java
try(Reader reader = Helper.openReader("filename.txt")) {
    // do stuff with reader
} catch(IOException e) {
    System.out.println(e.getClass().getSimpleName() + "-" + e.getMessage());
} finally {
    if(reader != null) {
        try {
            reader.close();
        } catch(IOException e2) {
            System.out.println(e2.getClass().getSimpleName() + "-" + e2.getMessage());
        }
    }
}
```

Also, a try-with-resources can be used with multiple resources.
Example: reading from file1.txt, then writing to file2.txt:

```Java
try(Reader reader = Helper.openReader("file1.txt");
    Writer writer = Helper.openWriter("file2.txt")) {
    while((length = reader.read(buff)) >= 0) {
        write.write(buff, 0, length);
    }
} catch(IOException e) {
    System.out.println(e.getClass().getSimpleName() + "-" + e.getMessage());
    for(Throwable t: e.getSuppressed()) { //When working with try-with-resources, we should use this to get any exceptions that were supressed for some reason
        System.out.println(t.getClass().getSimpleName() + "-" + t.getMessage());
    }
}
```

### Chaining Streams

Streams are usually chained together to create high-level functionality.
To chain streams, we make an instance of the high-level stream, and inside its constructor we pass in the low-level stream instance.

An example is InputStreamReader class, it provides Reader behavior over InputStream i.e. allows data to come in binary and get processed as characters.
When InputStreamReader is closed, it also closes the low-level stream.

```Java
void doChain(InputStream in) throws IOException {
    int length;
    char[] charBuff = new char[128];
    try(InputStreamReader rdr = new InputStreamReader(in)) { //notice we pass the reference to the InputStream in the constructor of rdr
        while((length = rdr.read(charBuff)) >= 0) {
            // do stuff with charBuff
        }
    } catch(IOException e) {
        //display e
    }
}
```

We can create our own high-level streams.
We usually chain similar streams like reader over a reader, etc.
Because this is common, Java provides abstract classes for us to conform to: FilterReader, FilterWriter, FilterInputStream, FilterOutputStream.
This helps us focus only on overriding the methods.

### File Streams & Buffered Streams

The Java.io packages have streams for files:

- FileReader.
- FileWriter.
- FileInputStream.
- FileOutputStream.
  These are deprecated, but are still used widely.

Interacting with files directly can be inefficient, so we need something else.
Buffered streams improve efficiency because they buffer the content in memory i.e. we can read and write large chunks, but reduce the interaction with streams themselves.
BufferedSteams are also useful beyond file streams, for example:

- Line breaks vary across platforms:
  _ Linux: \n
  _ Windows: \r\n
  Buffered streams support both and they determine the correct value for the current platform, we can use that for example:
- Buffered Writers: We can write line breaks with newLine().
- Buffered Readers: We can read line by line using readLine().

Types of buffer streams:

- BufferedReader.
- BufferedWriter.
- BufferedInputStream.
- BufferedOutputStream.

How to use readers:

```Java
try(BufferedReader br = newBufferedReader(new FileReader("file1.txt"))) {
    int intVal;
    while((intVal = br.read()) >= 0) {
        char c = (char) intVal;
        // do stuff with c
    }
} catch(IOException e) {
    // handle exception
}
```

How to use writers:

```Java
void writeData(String[] data) throws IOException {
    try(BufferedWriter bw = new BufferedWriter(new FileWriter("file1.txt"))) {
        for(String d: data) {
            bw.write(d);
            bw.newLine(); //will write each line from data on a separate line in the file
        }
    }
}

void readData() throws IOException {
    try(BufferedReader br = new BufferedReader(new FileReader("file1.txt"))) {
        String inValue;
        while((inValue = br.readLine()) != null) {
            System.out.println(inValue); //will print each line from the file in a separate line
        }
    }
}
```

#### Java.nio Classes For File Streams

Since Java.io file stream classes are deprecated, there are new ones in the Java.nio package.
These provide many benefits over Java.io:

- Better exception reporting.
- They deal with large files more efficiently.
- They support more features of the file system.
- Simplify a lot of common tasks.

#### Path and Paths classes

These are classes used to locate files and reference them:

```Java
Path p1 = Paths.get("path/to/file.txt");
Path p2 = Paths.get("path", "to", "file.txt");

// Both p1 and p2 point to the same file.
```

The Java.nio package provide Files and Paths classes which provide a lot of methods to simplify streams:
They contain factory methods that handle the creation of FileReaders, writers, etc.
Here's an example of using java.nio Files and Paths classes instead of BufferedReader and FileReader

```Java
void readData() throws IOException {
    try(BufferedReader br = Files.newBufferedReader(Paths.Get("file1.txt"))) {
        String inValue;
        while((inValue = br.readLine()) != null) {
            System.out.println(inValue); //will print each line from the file in a separate line
        }
    }
}
```

#### Specialized File Systems

Java can work with the default file system, or specialized systems like Zip file systems.
Paths class only works for default file systems, to interact with Zip file systems, we use:

- FileSystem class: it represents an individual file system, it's also a factory for Path instances.
- FileSystems class: it contains factory methods for FileSystem e.g. newFileSystem.

File systems are identified by URIs (Universal Resource Identifier, it's like URL of a website, actually URL is a type of URI).

How to create a zip file:

```Java
public static void main(String[] args) {
    String[] data = {
        "Line 1",
        "Line 2 2",
        "Line 3 3 3"
    };

    try(FileSystem zipFs = openZip(Paths.get("myData.zip"))) {  // openZip is a user-defined method, look below

    } catch(Exception e) {
        // handle exception
    }


private static FileSystem openZip(Path zipPath) throws IOException, URISyntaxException {
    // When we create a file system, we can specify some properties, to do this we'll use a map
    Map<String, String> providerProps = new HashMap<>();

    // for example, a boolean property 'create', when it's set it means that if the file system is not present, create it
    providerProps.put("create", "true");

    // when dealing with zip file systems, we put "jar:file" as a prefix to the URI used to get the file.
    // the path passed is converted to URI< which is then used to get the fully qualified path from it
    // forget the last parameter for now
    URI zipUri = new URI("jar:file", zipPath.toUri().getPath(), null);

    // now we have our URI and properties, we can create the file system using a FileSystems factory method.
    FileSystem zipFs = FileSystems.newFileSystem(zipUri, providerProps);
    return zipFs;
}
```

Now we'll try to copy a file to a zip file system, then create a new file inside the zip file system:

```Java
private static void copyToZip(FileSystem zipFs) throws IOException {
    // There are two ways to get the source file path:
    // Way 1: Path sourceFile = FileSystems.getDefault().getPath("file1.txt");
    // This is the long way, a short way would be the one we'll use.

    // Way 2:
    Path sourceFile = Paths.get("file1.txt"); //this is the file we'll copy
    Path destinationFile = zipFs.getPath("/file1Copied.txt"); //this is the copy we'll create

    Files.copy(sourceFile, destinationFile, StandardCopyOption.REPLACE_EXISTING);
    //The last argument is a prop saying if you find an already existing copy, overwrite it.
}

// How to create a file within a zip file system:
private static void writeToFileInZip(FileSystem zipFs, String[] data) throws IOException {
    try(BufferedWriter writer = Files.newBufferedWriter(zipFs.getPath("/newFile1.txt"))) {
        for(String d: data) {
            writer.write(d);
            writer.newLine();
        }
    }
}

private static void writeToFileInZip_2(FileSystem zipFs, String[] data) throws IOException {
    Files.write(zipFs.getPath("/newFile2.txt"),
        Arrays.asList(data),
        Charset.defaultCharset(),
        StandardOpenOption.CREATE); //these are options we pass

    // Why use Arrays.asList(data)? What if we didn't use Arrays.asList(data) and just passed data?
        // This will give us an error, because now data should implement Iterable, but String[] doesn't do that.
        // But, we can convert it to a LIST, which implements Iterable.
}
```

## String Formatting and RegExes

We'll learn more powerful ways to deal with strings, why?

- Just concatenation focuses on creation details.
- Awkward to use with numeric conversions.
- Even StringBuilder has these issues.

### String Formatting and Joinint

#### StringJoiner

It's a class specifically designed to simplify the process of creating a string from a sequence of values.
How to use it:

- Construct a StringJoiner.
  - Specify string to separate values i.e. a delimiter.
  - Specify start/end strings i.e. strings that will be put at the start and end of all the values you pass to the StringJoiner.
- Add values.
- Retrieve the resulting string.

```Java
StringJoiner sj = new StringJoiner(", ", "{", "}"); //we can pass a delimiter, a string to put at the start and at the end
sj.add("Moamen");
sj.add("is");
sj.add("Awesome");

// they can be chained btw
sj.add("Moamen").add("is").add("really").add("Awesome");

String result = sj.toString();
System.out.println(result); //Output: {Moamen, is, Awesome, Moamen, is, really, Awesome}

//The props we pass in the constructor can be used powerfully, for example:
StringJoiner sj2 = new StringJoiner("], [", "[", "]");
sj2.add("Sweet").add("Home").add("Alabama");

System.out.println(sj2.toString()); //Output: [Sweet], [Home], [Alabama]

// We can set a default value if the .add method was never called
sj2.setEmptyValue("EMPTY");
```

That is awesome xD

#### Format Specifiers

We use format specifiers to construct strings by focusing on the DESIRED result, not how it's constructed.

```Java
int a = 10, b = 20, c = 3.6666666666667;
String s1 = "I have " + a + " dollars, " + b + " cents, and " + c + " liters of gasoline.";
    //Output: I have 10 dollars, 20 cents, and 3.6666666666667 liters of gasoline.
String s2 = String.format("I have %d dollars, %d cents, and %.2f liters of gasoline.", a, b, c);
    //Output: I have 10 dollars, 20 cents, and 3.66 liters of gasoline.
```

There are a lot of details for string formatters, look them up in the docs, start here if you want a starting point:
https://docs.oracle.com/javase/tutorial/essential/io/formatting.html

Also check out _Format Flags_ and _Argument Index_, these are awesome.

#### Writing Formatted Content To Streams

Formatter class:

- Provides the formatting capabilities we've used all that time.
- Writes formatted contant to any type that implements Appendable interface e.g. StringBuilder, but also the Writer stream class.

So, we can use Formatter class to write formatted content to Writer class and any of its children.
That is awesome, we can write for example a formatted HTML contant to a network-based stream that inherits from Writer.

```Java
void doWrite(int a, b, c) throws IOException {
    BufferedWriter = Files.newBufferedWriter(Paths.get("myFile.txt"));
    try(Formatter f = new Formatter(writer)) { // formatter will automatically close at the end
        f.format("I have %d dollars, %d cents, and %.2f liters of gasoline.", a, b, c);
        //
    }
}
```

Check the Java Formatter documentation:
https://docs.oracle.com/javase/8/docs/api/Java/util/Formatter.html

---

### Regular Expressions in Java

We won't cover the whole topic of Regular Expressions (regex) here, because it's a big theoretical concept.
We'll look at methods that support using regex to match strings.

String class itself has some methods:

- replaceFirst, replaceAll: we give a pattern that describes which parts of the string we want to change.
- split: splits the string into an array of strings, we provide a pattern that describes the separator we'll detect to split at.
- match: identifies if a string matches a pattern we provide.

Examples:

```Java
String s1 = "Gimme gimme more, gimme more, gimme gimme more";

String s2 = s1.replaceAll("gim", "kim");
System.out.println(s2);  //Output: kimme kimme more, kimme more, kimme kimme more.

String[] s3 = s1.split("\\b"); // \\b is a word break i.e things between words either whitespace, comma, comma whitespace, etc.

for(String s: s3) {
    if(s.matches("\\w+")) { //"\\w"
        System.out.println(s); //Output (in new lines): gimme \n gimme \n more \n gimme \n more \n gimme \n gimme \n more
    }
}
```

Consider these when dealing with regex:

- The compilation of regex may be very processing intensive.
- String methods may lead to repeated calculations.

That's why some dedicated classes are introduced in Java:

- Pattern class:
  - Compiles a regex.
  - Factory for Matcher class instances.
- Matcher class:
  - Apply the compiled expression to a string.

```Java
String s1 = "Gimme gimme more, gimme more, gimme gimme more";

Pattern pattern = Pattern.compile("\\w+");
Matcher matcher = pattern.matcher(s1);

while(matcher.find()) {
   System.out.println(matcher.group()); //Output will be the same as the previous for loop
}
```

Check the Pattern and Matcher classes documentation.
https://docs.oracle.com/javase/8/docs/api/Java/util/regex/Pattern.html

Tutorial in regex:
https://docs.oracle.com/javase/tutorial/essential/regex/

---

## Collections

We know arrays (If you're now a noob at programming, that is).
Arrays are good for simple cases, but they have some limits:

- They are statically sized.
- They rely on explicit position management, we manage POSITIONS, not specific values.

Collections in Java have some advantages:

- Iterable, literally implementing Iterable interface.
- They provide type safety.
- They dynamically size.
- Some of them support ordering, prevent duplicates, manage data as name-value pairs, etc.

Collections use generics, which are specified during collection creation.
For example:

```Java
ArrayList<String> list = new ArrayList<>(); // We can remove the type from the <> after the new keyword.
list.add("Slim");
list.add("Shady");

for(String o: list) {
    System.out.println(o); // since we use generics and we specified that it's a string, the object o is returned AS a string.
}

// also, we can't add something that isn't a string
int a = 4;
list.add(a); // Will give an error
```

### Collection Interface

Collections implement the Collection interface (Except the Map collection).
Collection interface extends Iterable (which is why we can use the for each loop).
Common methods in the Collection interface:

- size()
- clear()
- isEmpty()
- add()
- addAll(): add all members from another collection

```Java
ArrayList<String> list1 = new ArrayList<>();
LinkedList<String> list2 = new LinkedList<>();

list1.add("Aperture science");
list1.add("Black Mesa");

list2.add(list1); // No problem, the members from list1 will be added to list2.
```

Many of the Collection interface methods rely on equality test, such ass:

- contains()
- containsAll(): does it contain all elements in another collection?
- remove()
- removeAll(): removes all elements contained in another collection.
- retainAll(): removes all elements not contained in another collection.

Let's look at removing a member:

```Java
public class MyClass {
    String label, value;

    public MyClass(String label, String value) {
        this.label = label;
        this.value = value;
    }

    public boolean equals(Object o) {
        MyClass other = (MyClass) o;
        return value.equalsIgnoreCase(other.value); // only if value fields are equal, both objects are declared equal.
    }
}
```

Now, let's create a collection of MyClass:

```Java
ArrayList<MyClass> list = new ArrayList<>();
MyClass v1 = new MyClass("v1", "a");
MyClass v2 = new MyClass("v2", "a");
MyClass v3 = new MyClass("v3", "a");

list.add(v1);
list.add(v2);
list.add(v3);

list.remove(v3);
// what will happen here is that
// 1. an equality test will be performed on each element until one returns true.
// 2. the first element that returns true is removed.
```

### Collection features added in Java 8

Java 8 allowed us to pass lambda expressions as arguments (i.e. pass code).
Collection methods that accept lambda expressions:

- forEach: perform the lambda expression on each member.
- removeIf: perform the lambda expression (which should be of return type boolean) on each member and removes it if it returns true.

```Java
ArrayList<MyClass> list = new ArrayList<>();
MyClass v1 = new MyClass("v1", "a");
MyClass v2 = new MyClass("v2", "b");
MyClass v3 = new MyClass("v3", "a");

list.add(v1);
list.add(v2);
list.add(v3);

// instead of for loop to print
// we can just do this:
list.forEach(m -> System.out.println(m.getLabel()));
list.removeIf(m -> m.getValue().equals('a'));

// They are very very similar to arrow functions in JS.
```

### Converting between Collections and Arrays

From Collection to Array:

- toArray(): returns array of objects.
- toArray(T[] array): returns array of type T.

````Java
``` Java
ArrayList<MyClass> list = new ArrayList<>();
MyClass v1 = new MyClass("v1", "a");
MyClass v2 = new MyClass("v2", "b");
MyClass v3 = new MyClass("v3", "a");

list.add(v1);
list.add(v2);
list.add(v3);

Object[] objArray = list.toArray();

MyClass[] typedArray1 = list.toArray(new MyClass[0]);
// if we pass an empty array of a certain type,
// the method will return a new array of that type

MyClass[] typedArray2 = list.toArray(new MyClass[3]);
// if we pass an array of a size sufficient for the elements in the collection,
// the method will return a REFERENCE to the array I passed in.
````

From Array to Collection:

- Arrays.asList()

```Java
MyClass[] a = {
    new MyClass("v1", "a"),
    new MyClass("v2", "a"),
    new MyClass("v3", "a")

};

Collection<MyClass> list = Arrays.asList(a);
```

### Common Collection Types in Java

#### Common collection interfaces:

- Colllection: basic collection operations
- List: maintains an order.
- Queue: an order with a specific 'head' element.
- Set: no duplicate values.
- SortedSet: no duplicates AND members are sorted.

#### Common collection classes:

- ArrayList:
  - Implements List.
  - Efficient for random access.
  - Inefficent for random insert.
- LinkedList:
  - Implements List and Queue, is implemented as a doubly linked list.
  - Efficient for random insert.
  - Inefficent for random access.
- HashSet:
  - Implements Set, is implemented as Hash table.
  - Efficient general purpose usage.
- TreeSet:
  - Implements SortedSet, is implemented as balanced binary tree.
  - Efficient in ordered access.
  - Inefficient in getting modified and searched than a HashSet.

#### How these Collections are Sorted

Sorting is specified in two ways:

- Implement the Comparable interface:
  - The Type specifies its own sorting behavior.
  - Therefore, the implementation of Comparable should be consistent with the implementation of the method 'equals'.
- Implement the Comparator interface:
  - Implemented by a certain Type that performs the sort (separate from the type of the collection that's getting sorted).
  - Specifies sort behavior of another type.

Let's start with a simple class:

```Java
public class MyClass {
    String label, value;
    public String toString() { return label + "|" + value; }
    public boolean equals(Object o) {
        MyClass other = (MyClass) o;
        return value.equalsIgnoreCase(other.value)l
    }
}
```

How to implement Comparable:

```Java
public class MyClass implements Comparable{
    String label, value;
    public String toString() { return label + "|" + value; }
    public boolean equals(Object o) {
        MyClass other = (MyClass) o;
        return value.equalsIgnoreCase(other.value)l
    }

    public int compareTo(MyClass other) {
        return value.compareToIgnoreCase(other.value); // Notice that we used 'value', the variable that's used in 'equals()'
    }
}
```

How to implement Comparator:

```Java
public class MyComparator implements Comparator<MyClass> {
    public int compare(MyClass x, MyClass y) {
        return x.getLabel().compareToIgnoreCase(y.getLabel());
        // Notice that we didn't have to use 'value' here
        // because we are using an external class to sort them
    }
}
```

How to sort a collection of MyClass using both methods:

```Java
// First method
TreeSet<MyClass> tree1 = new TreeSet<>();
tree1.add(new MyClass("2", "c"));
tree1.add(new MyClass("1", "b"));
tree1.add(new MyClass("3", "a"));

tree1.forEach(m -> System.out.println(m));

TreeSet<MyClass> tree2 = new TreeSet<>(new MyComparator()); // Notice we passed a new MyComparator object to be used
tree2.add(new MyClass("2", "c"));
tree2.add(new MyClass("1", "b"));
tree2.add(new MyClass("3", "a"));

tree2.forEach(m -> System.out.println(m));
```

So we can really specify how things are sorted, which is a really useful thing.

### Map Collections

Key/value pairs.

Map Interfaces:

- Map: basic map operations.
- SortedMap: sorted by keys.

Map Class:

- HashMap: efficient general purpose map.
- TreeMap: self-balancing tree, supports Comparable and Comparator interfaces for sorting.

Map methods:

- put(): add key and value.
- putIfAbsent(): put() if key isn't present or value is null.
- get(): return value of key, or null.
- getOrDefault(): get(), but instead of null it returns a specified default value.
- values(): returns a collection of all values.
- keySet(): returns a set of all keys (set because keys are unique).
- forEach()
- replaceAll()

SortedMap methods:

- firstKey()
- lastkey()
- headMap(): returns a MAP of all entries whose keys are less than (exclusive) a specified key.
- tailMap(): same but all keys are GREATER than OR EQUAL (inclusive) to a specified key.
- subMap(): returns a map of all entries whose keys are greater than or equal to (inclusive) a key, and less than (exclusive) another key i.e. it returns part of the original map in the key range that we specify, with the bottom range included.

```Java
Map<String, String> map = new HashMap<>();
map.put("2", "c");
map.put("3", "a");
map.put("1", "b");
```

---

## Controlling App Execution and Environment

In this module we'll look at how we can control our app state and the environment in which the app runs.
This is some _serious_ stuff, huh xD

### Command-Line Arguments

The bane of every developer's existence, although they are really easy.
We use them to pass startup info like:

- What to run.
- IO files & urls.
- Behavior options i.e. command switches.

Of course, these are passed in an array of strings to the main function, the famous String args[].
Each argument is a separate element, separated by the OS's whitespace, quoting is used for arguments WITH spaces in their names.

For example:

```Java
package com.jwhh.cmdline;

public class Main {
    public static void main(String[] args) {
        if(args.length < 1) {
            System.out.print("No arguments passed");
        } else {
            for(String a: args) {
                System.out.println(a);
            }
        }
    }
}
```

This is how we run it, passing some arguments:

```sh
$ java com.jwhh.cmdline.Main Hello There World
```

IDEs allow setting command line arguments in a way, so search for how to do it in a certain IDE like eclipse, IntelliJ, NetBeans, etc.

### Dealing with Persistent Key/Value Pairs

Persistent data is data that is NOT destroyed when the program exits, they're saved on the physical storage e.g. cookies, local storage, etc.
In java we can manage persistent data through storing it as key/value pairs, we need to:

- Set & retrieve values.
- Store and load it.
- Provide default values if not set.

Our hero this time is the Properties class.

- Inherits from HashTable class.
- Keys and values are strings.

Methods of Properties class:

- setProperty: sets or creates key with a new value.
- getProperty: gets value of key or null/default value if not found.

```Java
Properties props = new Properties();
props.setProperty("Name", "Moamen");
String name1 = props.getProperty("Name");
String name2 = props.getProperty("nextName", "defaultValue"); // We pass a default value to return if key isn't found
```

#### So how do you make this Properties object persistent?

They can be persisted using Streams.
They can be persisted in two formats:

- Simple text.
  - Using streams.
  - .properties extension.
  - ONE key/value pair per line.
    - Separated by = or :
    - Any whitespace around = or : is ignored.
    - Any OTHER whitespace becomes a separator.
    - The backslash' escapes whitespace.
    - Comments start with # or !.
    - Blank lines are ignored.
- XML.
  - Using streams.
  - .xml extension.
  - ONE key/value pair per XML element.
    - Element is named entry.
    - Key is stored as key attribute.
    - Value is stored as value of the element itself.
    - Comments are stored as Comment elements.

##### Simple text

```Java
Properties props = new Properties();
props.setProperty("Name", "Moamen");
props.setProperty("Nickname", "Mr. Awesome");

// Storing properties
try(Writer writer = Files.newBufferedWriter(Paths.get("moamen.properties"))) {
    props.store(writer, "My Comment");
    // props.store is the method we'll use
    // we pass any comments we want as the second argument.
    // the file will be created,
    // first line will be the comment, second line is date
    // then keys and values are displayed
}

// Loading properties
Properties props2 = new Properties();
try(Reader reader = Files.newBufferedReader(Paths.get("moamen.properties"))) {
    props2.load(reader);
}

String val1 = props2.getProperty("Nickname");
System.out.println(val1); // Output: Mr. awesome
```

If there are spaces in the value in the text file (other than the ones around = or :) (Hey that looks like a smily face), they might cause problems.

##### XML

```Java
Properties props = new Properties();
props.setProperty("Name", "Moamen");
props.setProperty("Nickname", "Mr. Awesome");

// Saving properties
try(Writer writer = Files.newBufferedWriter(Paths.get("moamen.xml"))) {
    props.storeToXML(writer, "My Comment");
    // The XML file will have first the version and DOCTYPE
    // then the top element <properties>
    // then our comment in <comment>
    //  <entry key="Name">Moamen</entry>
    // etc.
}

// Loading properties
Properties props2 = new Properties();
try(Reader reader = Files.newBufferedReader(Paths.get("moamen.xml"))) {
    props2.loadFromXML(reader);
}

String val1 = props2.getProperty("Nickname");
System.out.println(val1); // Output: Mr. awesome
```

#### Default Values

We pass default Properties to the constructor of Properties.
These are searched if key is not found.
Default values passed in the constructor _take precedence_ over default values passed in getProperty.

```Java
Properties defaults = new Properties();
defaults.setProperty("Name", "Moamen");

Properties props = new Properties(defaults);
String p1 = props.getProperty("Name"); //Output: Moamen

props.setProperty("Name", "Moamen Moataz");
String p2 = props.getProperty("Name"); //Output: Moamen Moataz
```

Defaults are usually decided on when starting the development, so:

- Create the default property file as part of the PACKAGE of your app.
- Create .properties file at dev time.
- Build process will include it in the package.
- So now we can load it using getResourceAsStream method for example (Which is a part of something called Java Resource System).
- Any class can access properties now.

## Class Loading

This topic has frustrated me so many times because of the freaking classpath, so pay attention so as not to pass through the pain that I've been through :'D

Most apps don't stand alone, they rely on other classes in other packages.
JDK packages are located automatically.
But what about other packages?

We solve this by:

- First of all, each IDE loads packages at dev time on its own.
- Default, java searches the CURRENT directory:
  - All classes must be in .class files.
  - And must be under package directories that match their packages.
    e.g. if a class is in package .com.pluralsight.training, then it must be in directory ./com/pluralsight/training.
- Or, we can specify a specific location or locations for Java to search for classes in.
  - These are searched in the order they appear in.
  - Once we do that, the current directory is excluded UNLESS we put it into the list.

How to specify these paths? Two ways:

1. By an environment variable, called CLASSPATH. - When we set it, it becomes a DEFAULT PATH, which means, if a Java app doesn't specify some paths for itself, it will use the current value of CLASSPATH to search for classes. - That's why, we should use this with caution, some programs may depend on its value, so changing it might lead to problems, even adding paths to it may lead to name collisions and stuff.
   Search for how to set an environment variable according to your OS, but generally:

```sh
#Windows
$ set CLASSPATH="path/you/want"
```

Class Path Structure:

- Windows: path1 ; path2 ; path 3 ...
  - Separated by ;
- Unix: path1 : path2 : path3 ...
  - Separated by :
- .class files: Path to folder containing package root.
- .jar file: path to the file, inluding its name

2. Passing the classpath for the java app when running:

```sh
#Windows
$ java -cp \path1;\path2 com.pluralsight.training.Main

#Unix
$ java -cp /path1:/path2 com.pluralsight.training.Main
```

Search for how to do this for .jar files, and using the -jar command line option.

### Execution Environment Information

Apps often need info like:

- User info.
- System info.
- Java config info.
- App specific info.

To do this:

- System Properties.
  - Info that Java provides about the environment:
  - Accessed by System.getProperty.
  - Info include: User info, Java installation info, OS config info.
- Environment Variables.
  - OS provides these variables.
  - They provide config info.
  - Are mostly automatically set by OS.
  - But can provide app-specific variables.
  - Accessed by System.getenv(): you'll get a Map object containing environment variables.
  - Or System.getend(name): get only the value of the environment variable whose name you pass as argument.

---

## Capturing App Activity with the Log System

Why do we need a log system at all?

- To capture app activity.
- To capture unusual circumstances and errors.
- Track usage info.
- Debugging.

The required level of detail in terms of logging can vary:

- A newly deployed app, or an app experiencing errors, may require a LOT of logging details.
- An app that is mature and stable needs little details.

java.util.logging Package gives us a lot of these features.

The log system is centrally managed:

- Only one logging instance per app.
- It manages log system config.
- And manages the objects that do the actual logging.

LogManager class:

- One global instance.
- LogManager.getLogManager() returns a reference to that instance.

Logger class:

- Provides logging methods.
- We get access to a Logger using LogManager class by getLogger().
- Each instance is named.
- A global logger instance is available.
- And is accessed by a Logger class static field called GLOBAL_LOGGER_NAME

```Java
public class Main {
    public static void main(String[] args) {
        LogManager lm = LogManager.getLogManager(); // lm now has reference to log manager global instance
        Logger logger = lm.getLogger(Logger.GLOBAL_LOGGER_NAME);
        logger.log(Level.INFO, "My logger message"); // each logger entry has a LEVEL, we'll see that later.
        logger.log(Level.INFO, "My second logger message");
    }
}
```

Because in order to access a logger we need to perform the first two statement over and over,
in practice we do something different:

```Java
public class Main {
    // we make the logger STATIC
    static Logger logger = LogManager.getLogManager().getLogger(Logger.GLOBAL_LOGGER_NAME);

    public static void main(String[] args) {
        // now we can use the logger in any method inside the class
    }
}
```

### Logging Levels

Logging level control how deep do we go into details.
Also, each logger has a Capture Level, we set it using setLevel.
The logger will IGNORE any level argument passed that's LESS than its own capture level.

Each level has a numeric value:

- 7 basic log levels.
- 2 special levels for a logger.
- We can define custom levels, but that's generally avoided.

Logging levels:
(Format: LEVELNAME: level_value, level_meaning).

- SEVERE: 1000, serious failure.
- WARNING: 900, potential problem.
- INFO: 800, general info. (That's what we passed when we passed Level.INFO in the logger.log() function)
- CONFIG: 700, config info.
- FINE: 500, general dev info.
- FINER: 400, detailed dev info.
- FINEST: 300, specialized dev info.

- ALL: Integer.MIN_VALUE, Logger capture EVERYTHING
- OFF: Integer.MAX_VALUE, Logger doesn't capture anything.

Logging methods:

- log(): we saw that.
- level conveninece levels: we actually have methods that identify the level for us and we only pass the message
  e.g. severe(), warning(), fine(), etc.
- Precise log method:
  We specify the class name and method we want to log.
  logp(Level.SEVERE, "com.pluralsight.training.Main", "myMethod", "Oh my goodness!");
- Precise convenience methods:
  These log predefined messages: (Their level is FINER)
  So, we need to set the level so that it contains the FINER level (because it's not included by default)
  .entering("com.pluralsight.training.Main", "myMethod");
  .exiting("com.pluralsight.training.Main", "myMethod");

Check out _Parametrized log methods_.

### Log System in More Details

The log system has different components.
Each component handles a specific tasks.

There are mainly 3 core components:

- Logger: accepts out method calls.
- Handler: responsible for publishing logging info, a Logger can have many Handlers.
- Formatter: formats log info for publication e.g. text, XML, etc., each Handler has 1 Formatter.

Each logger has a setLevel.
Handlers also have setLevels, they can be more restrictive than the logger.

How to create/add log components:

- Create a logger: Logger.getLogger() static method.
- Adding a Handler: Logger.addHandler().
- Adding a formatter: Handler.setFormatteR(). (Because a handler gets ONE formatter).

```Java
public class Main {
    static Logger logger = Logger.getLogger("com.pluralsight");
    public static void main(String[] args) {
        Handler h = new ConsoleHandler();
        Formatter f = new SimpleFormatter();

        h.setFormatter(f);
        logger.addHandler(h);
    }
}
```

### Built-in Handlers

We can add our custom handlers, but that's usually not necessary.
Built-in handler:

- ConsoleHandler: writes to System.err
- StreamHandler: writes to an output stream.
- SocketHandler: writes to a network socket.
- FileHandler: writes to one or more files.
  This handler can write to ONE file, or many rotating files.
  This means:
  - We can specify the max size of a file in bytes.
  - We can specify the max number of files.
  - We can reuse oldest files.

Check out how to use FileHandler with many files.

### Built-in Formatters

Formatter class is the parent class for both:

- XMLFormatter.
- SimpleFormatter.
  - Formats content as simple text.
  - Format is customizable using the formatting notation we covered earlier.

To be seen:

1. _Built-in Formatters_
2. _Log Configuration File_
3. _Making the most our of the Log system_

## Multithreading In Java

A process is an instance of a program or app.
It has resources like Memory, RAM, etc.
It has at least ONE thread.

What's a thread?

- A sequence of program instructions.
- It's that thing that actually executes a program's code.
- It uses resources to do that.
  At any moment of time while the program is running, its thread is interacting with a certain resource.

Of course, a program can have many threads, not just one i.e. many threads executing your code.
That is fine as long as at any point of time, no two or more threads use the same resource, this is called Concurrency.

Why would we do that?

- Threads often wait for non-cpu tasks e.g. interacting with storage, networks, etc.
- Many PCs now have multiple CPU cores.
- Threads help us use the CPU more efficiently.
- We can reduce the wall-clock execution time (Although it takes MORE CPU cycles, but that's because many threads are working).

### How to Make our Java Programs Multithreaded

Let's begin with a class Adder, it takes a file as input, adds all values written in it, and writes the result in another file.

```Java
class Adder {
    private String inFile, outFile;

    public Adder(String inFile, String outFile) {
        // assign
    }

    public void doAdd() throws IOException {
        int total = 0;
        String line = null;
        try(BufferedReader reader = Files.newBufferedReader(Paths.get(inFile))) {
            while((line = reader.readLine()) != null) {
                total += Integer.parseInt(inLine);
            }
        }

        try(BufferedWriter writer = Files.newBufferedWriter(Paths.get(outFile))) {
            writer.write(total);
        }

        // Since we use files and stuff, opening the files, reading from them, writing to them, interacting with disks, etc.
        // all of these are processes that don't use CPU at least as much as the rest i.e. our thread is waiting.
    }
}

public class Main {
    public static void main(String[] args) {
        String[] inFiles  = // a list of filenames
        String[] outFiles = // a list of filenames

        try {
            for(int i = 0 ; i < inFiles.length ; i++) {
                Adder adder = new Adder(inFiles[i], outFiles[i]);
                adder.doAdd();
            }
        } catch(IOException e) {
            // handle the IOException
        }
    }
}
```

Now, this is a single-thread app, it will doAdd for file1, then doAdd for file2, all the way till the end, then it will do cleanup work.

Now, we want to fire a NEW THREAD for every file, so that they're all executing concurrently, we'll take much less time.

### Threading In Java

Jave Threads:

- They are very similar to OS threads.
- Each thread has a task, when it finishes, it terminates.
- Exceptions are tied to each thread.
- Any coordination between them is handled by us.

We usually use two things: Runnable interface, Thread class.

1. Runnable interface:

   - It represents a task to be run on a thread.
   - It has only run() method.

2. Thread class:
   - Represents a thread of execution.
   - It starts executing WHEN WE CALL THE start() method.

Now, let's convert our example to multithreaded.
First, let's implement the Runnable interface by class Adder:

```Java
class Adder implements Runnable {
    private String inFile, outFile;

    public Adder(String inFile, String outFile) {
        // assign
    }

    public void doAdd() throws IOException {
        // do stuff that we specified last time
    }

    public void run() {
        try { // because each THREAD sees its own exceptions
            doAdd();
        } catch(IOException e) {
            // handle exception
        }
    }
}
```

Now let's modify the main function:

```Java
public class Main {
    public static void main(String[] args) {
        String[] inFiles  = // a list of filenames
        String[] outFiles = // a list of filenames

        for(int i = 0 ; i < inFiles.length ; i++) {
            Adder adder = new Adder(inFiles[i], outFiles[i]);
            Thread thread = new Thread(adder); // The constructor expects a type that implements Runnable.
            thread.start(); //Now the thread will run the run() method inside the type that was passed to it.
        }
    }
}
```

This code will take care of running each loop iteration in a thread, but..
Once all the threads are starting, the main thread now has nothing to do,
it MAY terminate even BEFORE the created threads finish.
Once the main thread terminates, the entire process gets cleaned up.

So, we need to make sure the background work is done.
i.e. the main thread WAITS for all the other threads to finish.

To do this:

```Java
public class Main {
    public static void main(String[] args) {
        String[] inFiles  = // a list of filenames
        String[] outFiles = // a list of filenames

        Thread[] threads = new Thread(inFiles.length); // We declared the threads OUTSIDE the loop

        for(int i = 0 ; i < inFiles.length ; i++) {
            Adder adder = new Adder(inFiles[i], outFiles[i]);
            threads[i] = new Thread(adder);
            threads[i].start();
        }

        for(Thread thread: threads) {
            thread.join(); // this method will make the calling thread BLOCK until the background thread finishes.
        }
    }
}
```

Now, we have a reference for all the threads outside the for loop i.e. we can store these references.
And the join() loop will make the main thread wait for all the other threads.

That, is awesome ^\_^

### Thread Management Details in Java

The Thread class is awesome, but...
If you're not a seasoned threading programmer, it's really easy to misuse threads.
Plus, we really don't want to get into all of these details anyway.

Out comes, Java, our hero, providing abstractions.
Thread Pools: these are a queue of tasks, Each task in that queue will be assigned to its own thread from a pool of threads.

- ExecutorService interface:
  - Models thread pool behavior.
  - We can submit tasks into the pool.
  - Request and wait for pool shutdown.
- Executors class:
  - It has methods for creating thread pools.
  - The pools can be dynamically sized.
  - We can schedule tasks for later.

Let's look at the code we used before.
This code is NOT scalable, if we have 1000 files, and we start 1000 threads, the overhead of managing them and coordinating disk access will be nightmarish, it will make the CPU die probably.
So, we can use that to try thread pools:

```Java
public class Main {
    public static void main(String[] args) {
        String[] inFiles  = // a list of filenames
        String[] outFiles = // a list of filenames

        ExecutorService es = new Executors.newFixedThreadPool(3); // We pass a max number of threads value

        for(int i = 0 ; i < inFiles.length ; i++) {
            Adder adder = new Adder(inFiles[i], outFiles[i]);
            es.submit(adder);
            // the es will take care of assigning adder to a queue,
            // then to a thread (if available i.e. not more than 3 threads),
            // then run the thread
        }

        try {
            es.shutdown(); // We're requesting the shutdown of the pool
            es.awaitTermination(60, TimeUnit.SECONDS);
        } catch(Exception e) {
            // handle exception
        }
    }
}
```

### Creating Closer Relationship Between Thread Tasks

A lot of times we want the threads to interact with each other, pass results to each other, and throw exceptions to each other.
For example, we want a side thread to return the result to the main thread.
And if it raises an exception, that exception is received by the main thread.

To do that, we use types:

- Callable interface.
  - Very similar to Runnable, but it CAN return results AND THROW exceptions.
  - Only member is the call() method.
- Future interface:
  - Represents the RESULT returned from a thread.
  - it's returned by ExecutorService.submit.
  - It has get() method that:
    - Blocks until the thread finishes.
    - Return the result by the Callable.
    - Throw exception by the Callable.

In our Adder class, in method doAdd, instead of writing the result to a file, we'll RETURN IT.

1. instead of writing, we'll return the Total value.
2. we'll remove outFile from the class.
3. we'll change the return type of doAdd to int.
4. we'll implement Callable.
5. replace run() with call().

```Java
class Adder implements Callable { // Step 4 here
    private String inFile; // Step 2 here

    public Adder(String inFile) {
        // assign
    }

    public int doAdd() throws IOException { // Step 3 here
        int total = 0;
        String line = null;
        try(BufferedReader reader = Files.newBufferedReader(Paths.get(inFile))) {
            while((line = reader.readLine()) != null) {
                total += Integer.parseInt(inLine);
            }
        }

        return total; // Step 1 here

    }

    public Integer call() throws IOException { // Step 5 here
        return doAdd();
    }
}
```

Now, our Adder class runs in a separate thread, and return the results back including any exceptions it might throw.
Now, let's look at the Main code:

1. Get rid of the outFiles.
2. Make an array of Future to retrieve the results of the background threads.
3. Make that array receive the values returned from the background threads.
4. Use Future's method get() to get the values.

```Java
public class Main {
    public static void main(String[] args) {
        String[] inFiles  = // a list of filenames
        // Step 1 here

        ExecutorService es = new Executors.newFixedThreadPool(3);

        Future<Integer>[] results = new Future[inFiles.length]; // Step 2 here

        for(int i = 0 ; i < inFiles.length ; i++) {
            Adder adder = new Adder(inFiles[i], outFiles[i]);

            results[i] = es.submit(adder); // Step 3 here
        }

        for(Future<Integer> result: results) {
            try {
                int value = result.get(); // This get() blocks until the return value is available i.e. the thread has finished
                System.
            } catch(ExecutionException e) { // Notice that the exception is not an IOException, so we need to get that
                Throwable adderEx = e.getCause(); // This method returns the exception that happened in the background thread
                // handle adderEx
            } catch(Exception e) {
                // handle other exceptions
            }
        }

        try {
            es.shutdown();
            es.awaitTermination(60, TimeUnit.SECONDS);
        } catch(Exception e) {
            // handle exception
        }
    }
}
```

### Concurrency

If threads share common resources:

- If they are reading them only, there's no problem here.
- If they write into those common resources, that could lead to problems:
  - Some threads may receive wrong results.
  - Crashes can happen.

Let's look at this:

```Java
class BankAccount {
    private int balance;

    public BankAccount(int balance) {
        this.balance = balance;
    }

    public int getBalance() {
        return this.balance;
    }

    public void deposit(int amount) {
        balance += amount;
    }

}

public class Worker implements Runnable {
    private BankAccount account;

    public Worker(BankAccount account) {
        this.account = account;
    }

    public void run() {
        for(int i = 0 ; i < 10 ; i++) {
            int startBalance = account.getBalance();
            account.deposit(10);
            int endBalance = account.getBalance();
        }
    }

    public static void main(String[] args) {
        ExecutorService es = Executors.newFixedThreadPool(5);
        BankAccount account = new BankAccount(100);

        for(int i = 0 ; i < 5 ; i++) {
            Worker worker = new Worker(account);
            es.submit(worker);
        }
        // Shutdown es and wait
    }
}
```

If each thread adds 10 dollars 10 times, we would end with 600, or 580, or 590, something random.
You know why?
Because a thread can be writing a value of 110 for example, AT THE SAME TIME THAT another thread is reading the balance 100
So, now the second thread will add to 100,
but if a new thread reads and adds, it will add to 110,
the second thread may try to write 110, while the third thread is trying to write 120.

Note: Use the logging system to check that out, make each thread log the starting and ending balance and its number.

#### What's happening?

balance += account;
This instruction is actually many instructions:

1. Read current balance value.
2. Perform the arithmetic operation.
3. Write back the value to memory.

This is known as a _Non-atomic operation_ i.e. an operation that has many steps, and these steps don't happen at one time.
So, it's possible that changes happen from thread 2 while thread 1 is IN THE MIDDLE OF executing these steps.
e.g.
The value is 110, Thread 3 reads it, adds 10, then writes back, but
while it's writing back, thread 4 has already read the value (110) and is performing step 2 which is adding.
So now, Thread 3 will write back 120 and instead of Thread 4 reading the new value 120, adding to it, then writing 130,
Thread 4 actually HAS already read the old value 110 BEFORE thread 3 writes back 120, so now Thread 4 will write back 120 again!

This is known as a _Race condition_.
So, We need to Coordinate the accessing of this particular value, this _shared resource_.

### How to Solve Race Condition

Java gives us ways to solve this:

1. Synchronized Methods: coordinate thread access to methods,

   - This is done by adding 'synchronized' modifier.
   - A class can have as many sync methods as it needs.
   - Actually, this is managed at the CLASS instance level, not the object instance level.
     i.e. if a thread accesses a sync method in a class, no other thread can call ANY OTHER SYNC METHOD in that same class.

   - When to use this?

     1. To prevent modification by multiple threads. (i.e. race condition)
     2. Reading values that are being modified by another thread.

   - Cons?

     1. Has SIGNIFICANT overhead.
     2. ONLY use in multithreaded scenarios.

   - Constructors are NEVER synchronized, constructors create instances, and an instance should ALWAYS be created on ONE thread.

Let's go back to our example, we'll make the method deposit() synchronized:

```Java
class BankAccount {
    ...

    public synchronized int getBalance() {
        return this.balance;
    }
    // Why this sync? so that if a thread calls getBalance(), no other thread can call getBalance OR deposit.

    public synchronized void deposit(int amount) {
        balance += amount;
    }
}

// If we stop here, and run the code, we'll always get 600 as the result, ALWAYS 600, no 590, no 580, no random stuff,
// we'll ALWAYS get 600.
// So that means now the methods are synchronized, they access methods one at a time.

public class Worker implements Runnable {
    private BankAccount account;

    public Worker(BankAccount account) {
        this.account = account;
    }

    public void run() {
        for(int i = 0 ; i < 10 ; i++) {
            int startBalance = account.getBalance();
            account.deposit(10);
            int endBalance = account.getBalance();
        }
    }

    public static void main(String[] args) {
        ExecutorService es = Executors.newFixedThreadPool(5);
        BankAccount account = new BankAccount(100);

        for(int i = 0 ; i < 5 ; i++) {
            Worker worker = new Worker(account);
            es.submit(worker);
        }
        // Shutdown es and wait
    }
}
```

How does sync work?

- Before a thread calls a method or value, it makes a CHECK to see if that thing is LOCKED.
- If it's NOT LOCKED, the thread goes in and ACQUIRES the lock.
  - After it finishes, it RELEASES the lock.
- If it's LOCKED, it blocks until the lock is RELEASED.

### How to Protect Code That Runs On Multiple Method Calls (Manual Synchronization)

Not only sync methods have locks, ALL JAVA OBJECTS HAVE LOCKS!
That means, we can manually acquire and release locks :o

We do this using a Synchronized statement block.
This is useful because ANY code that has a reference to an object can use the Synchronized statement block to lock that object.

#### Synchronized Methods
Look at this, our old code:
```Java
class BankAccount {
    ...

    public synchronized void deposit(int amount) {
        balance += amount;
    }
}

class Worker implements Runnable {
    private BankAccount account;

    public void run() {
        for(int i= 0 ; i < 10 ; i++) {
            account.deposit(10);
        }
    }
}
```
This is thread-safe because deposit and getBalance are synchronized.
We can make this thread-safe in another way, by using Synchronized statement blocks:
``` Java 
class BankAccount {
    ...

    // We removed the synchronized keyword
    public void deposit(int amount) {
        balance += amount;
    }
}

class Worker implements Runnable {
    private BankAccount account;

    public void run() {

        // Look here, see how we use synchronized statement blocks
        // we pass an object to lock it, in our case we passed
        // the reference to the BankAccount
        synchronized(account) {
            for(int i= 0 ; i < 10 ; i++) {
                account.deposit(10);
            }
        }
    }
}
```

Why use synchronized blocks?
- More flexible, can be used in non-thread safe classes.
- Can protect complex BLOCKS of code (we'll see that next).
- Sometimes sync methods aren't enough.

#### Manual Synchronization Example
Let's look at the bank account class and add a method, withdrawal:
```Java
class BankAccount {
    ...

    public synchronized int getBalance() {
        return this.balance;
    }

    public synchronized void deposit(int amount) {
        balance += amount;
    }

    public synchronized void withdrawal(int amount){
        balance -= amount;
    }
}
```

Let's create another class, TxWorker, responsible for doing exactly ONE transaction:
``` Java
public class TxWorker implements Runnable {
    protected BankAccount account;
    protected char txType;
    protected int amt;

    // constructor

    public void run() {
        if(txType == 'w') {
            account.withdrawal(amt);
        } else if(txType == 'd') {
            account.deposit(amt);
        }
    }
}
```
Now, that is safe, even if one account has two txworkers working at the same time, because they're synchronized, it's thread-safe.

Now, we can dispatch these workers on a thread pool:
``` Java
ExecutorService es = Executors.newFixedThreadPool(5);

TxWorker[] workers = // retrieve txworker instances

for(TxWorker worker: workers) {
    es.submit(worker);
}

// shudown es and await
```
Now, let's say that a requirement is added:
If anyone makes a deposit, and the balance exceeds 500 after the deposit, the bank will give 10% bonus from the (balance - 500).

So, to solve this, we'll create a worker to do this promotion:
``` Java
public class TxPromoWorker extends TxWorker {
    // constructor that calls super(...)

    public void run() {
        if(txType == 'w') {
            account.withdrawal(amt);
        } else if(txType == 'd') {
            account.deposit(amt);
            if(account.getBalance() > 500) {
                int bonus = (int) ((account.getBalance()) - 500) * 0.1;
                account.deposit(bonus);
            }
        }
    }
}
```

Still, this is safe, because all the account's methods are synchronized.

Then, a very small modification to the driver code:
``` Java
ExecutorService es = Executors.newFixedThreadPool(5);

// Here
TxWorker[] workers = // retrieve toPromoWorker instances
// That's it?

for(TxWorker worker: workers) {
    es.submit(worker);
}

// shudown es and await
```

What if, After a worker deposits, BUT BEFORE IT gets balance amount, another one locks and executes withdrawal and leaves balance at 300, then the bonus is added?
This means, the getBalance and bonus will be calculated on the NEW balance (300), that'll lead to a lot of wrong calculations!

So, we need to write our code in a way that all the methods here:
``` Java
account.deposit(amt);
if(account.getBalance() > 500) {
    int bonus = (int(account.getBalance()) - 500) * 0.1;
    account.deposit(bonus);
}
```
We meed these methods to happen together.

The solution to this, is to use a synchronized block (Manual sync) to MANUALLY lock the account, then run all those methods in one block:
``` Java
public class TxPromoWorker extends TxWorker {
    // constructor that calls super(...)

    public void run() {
        if(txType == 'w') {
            account.withdrawal(amt);
        } else if(txType == 'd') {

            // see here? we used a synchronized block to lock the account manually
            // so that we can execute everything inside this block atomically
            synchronized(account) {
                account.deposit(amt);
                if(account.getBalance() > 500) {
                    int bonus = (int) ((account.getBalance()) - 500) * 0.1;
                    account.deposit(bonus);
                }
            }
            // end of synchronized block
        }
    }
}
```

### Other types used for Concurrency
Collections:
- Most collections are NOT thread-safe.
- We can do that manually, as we did before, but
- Jave provides a Synchronized Collection Wrappers
- The Collection class has static factory methods that create synchronized wrapper collections e.g. synchronizedList, synchronizedMap, etc.
- All the collection work occurs in the ORIGINAL collection, but you're accessing it through thread-safe wrappers.

Blocking queues:
- If someone tries to read blocks from a blocking queue while it's empty, it will be blocked until it has something.

Other stuff provided by Java:
- java.util.concurrent
    - it has Runnable, Callable, Executors, and it has other types that are advanced like Semaphores class.
- java.util.concurrent.atomic:
    - Provide types that has atomic operations:
        - atomicBoolean, etc.
        - set, get, getAndAdd, compareAndSet, and more methods.

---

## Type info and Reflections

Reflections are things that let us:

- Examine types at runtime.
- Dynamically execute and access members of types.

Why?

- Apps don't always control the types used.
  - This is common for advanced app designs.
  - Also common if you're developing a framework or a tool.
- That's because your code may need to interact with different types.
- Also, you may need to deal with legacy databases and stuff, you don't know what to expect.
- So you can't just write type-specific source code.
- So, sometimes we need to DYNAMICALLY LOAD types.

We can fully examine objects at runtime, we can determine an object's:

- Type and base type.
- Which interfaces it implements.
- Members.

We can also use reflections on a type to:

- Construct instances of a type.
- Access its fields.
- Call its methods.

This helps us make apps that are Configurable, the specific tasks are externally control.
This follows the design principle: inversion of control.
It means that an app provides some fundamental behavior, then external developers can add their specialized behavior through classes.

### Types as Types

Types are the foundations of any app solution, we use them to:

- Model business objects e.g. class Car, etc.
- Model technical issues e.g. Runnable, Thread, all that.

When we make an object instance, we create an instance of the object's class.
When we create a class, it's actually an object instance of another class called '_Class_'.

For every type we have in our app, there's an instance of _Class_ that describe it.

Let's look at some code:

```Java
public class BankAccount {
    private final String id;
    private int balance = 0;

    public BankAccount(String id) {...}
    public Bankaccount(String id, int balance) {...}

    public String getId() {...}
    public synchronized int getBalance() {...}
    public synchronized void deposit(int amount) {...}
}

// There is an instance of *Class* class, it models the type BankAccount we just wrote,
// this *Class* instance has members, like:
// 1. simpleName: BankAccount
// 2. fields: a collection that has all the fields, methods, and constructors,
//      it has an entry for id, an entry for balance, an entry for each constructor and each method
```

This _Class_ instance is actually accessible at run-time.
So, we have a model to work with our bank account class.
Every time we create a BankAccount instance, the _Class_ instance that represents Bankaccount is
used as a MOLD for the memory allowed for an instance of BankAccount.

The relationship between the _Class_ instance and the Type(BankAccount) instance is not just limited to creation of instances.
Each Type instance has a reference to the _Class_ instance that describe it.

So, we can create 1000 instances of the Type, and they all reference ONE INSTANCE of _Class_, the one that describes the Type.

This means, we can refer to any object without knowing what type that object actually is, because we can get to the _Class_ instance that describes it, and USE that _Class_ instance to construct other instances of that object.

### How to access a type's _Class_ instance

If we have an instance of a given type, we can use getClass() method to get the _Class_ instance that describes it.
If we have only the type's NAME as a string, we can use Class.forName() static method and pass the type's name as long as it's a fully qualified name (i.e. with packages and all that).
We can also use typename.class on the type's name, like String.class for example.

```Java
void showName(Class<?> theClass) { // Notice we have a generic type '?'
    System.out.println(theClass.getSimpleName());
}

// The function accepts a *Class* reference, that describes a given type e.g. BankAccount
// So here we should pass the *Class* instance that describes BankAccount.
// But the parameter has a generic <?>, it can specialize the type it operating against.
// In many cases we don't know the type that's going to be passed, that's why we put the generic argument.

void doWorkCase1(Object o) {
    Class<?> c = obj.getClass();
    showName(c);
}

// This function above can accept any type, then get its *Class* instance, then passes that to the showName function.
// It accepts an object reference, which is the first case that we described.

void doWorkCase2(String fqn) { // fqn as in Fully Qualified Name, something like "com.pluralsight.tutorial.BankAccount"
    Class<?> c = Class.forName(fqn);
    showName(c);
}
```

Both ways to access the _Class_ instance return the same reference to the same _Class_ instance, the same description of the type BankAccount.

### How to access a Type's info

Once we access the _Class_ instance that describes a Type, we can know:

- Its superclass, if it has one.
- Interfaces,
- Modifiers,
- Members,
  We've got a lot of power now :'D

```Java
public final class HighVolumeAccount extends BankAccount implements Runnable {
    // constructors
    public int[] readDailyDeposits() {...}
    public String run() {
        for(int depositAmt: readDailyDeposits()) {
            deposit(depositAmt);
        }
    }
}

void classInfo(Object obj) {
    Class<?> c = obj.getClass();
    System.out.println(c.getSimpleName());
    Class<?> superclass = c.getSuperclass();    // a reference to the *Class* instance that describes its superclass
                                                // we can do that again to the superclass,
                                                // we can walk all the way up the hierarchy and get the info of all that
    Class<?>[] interfaces = c.getInterfaces();  // same thing, we get *Class* instances that describe all interfaces
                                                // implemented by that class
    for(Class<?> interface: interfaces) {
        System.out.println(interface.getSimpleName());
    }
}
```

We can also use the method getModifiers(), this returns ONE integer value, why?
Each modifier is represented by a simple bit inside that integer, so, we need to interrogate that integer to know the modifiers, we can use Modifier class to interpret that integer.

For example:

```Java
void typeModifiers(Object obj) {
    Class<?> c = obj.getClass();
    int modifiers = c.getModifiers();

    // if((modifiers & Modifier.FINAL) > 0) {...} // this is a bitwise comparison
    if(Modifier.isFinal(modifiers)) { //this is much easier
        ...
    }

    if(Modifier.isPrivate(modifiers)) {
        ...
    }

    // Other methods are isProtected, isPublic, etc.
}
```

### How to access a Type's members

We can access a Type's DECLARED members, all of them, public, private, protected, etc. using:

- getDeclaredFields()
- getDeclaredMethods()
- getDeclaredConstructors()

We can access a Type's Declared AND inherited members, ONLY THE PUBLIC ONES.

- getFields()
- getMembers()
- getConstructors()
  There's something, Constructors are NOT inherited, so getConstructors just shows the public declared constructors of the Type.

```Java
// Using the class BankAccount

void fieldInfo(Object o) {
    Class<?> c = o.getClass();

    Field[] fields = c.getFields();
    for(Field f: fields) {
        System.out.println(f);
    }


    Field[] declaredFields = c.getDeclaredFields();
    for(Field f: declaredFields) {
        System.out.println(f);
    }
}


void methodInfo(Object o) {
    Class<?> c = o.getClass();

    Method[] methods = c.getMethods();
    for(Method m: methods) {
        System.out.println(m);
    }

    Method[] declaredMethods = c.getDeclaredMethods();
    for(Method m: declaredMethods) {
        System.out.println(m);
    }
}

```

Using getFields and getMembers will get all declared and inherited methods, all the way up the hierarchy to Object class.
What if we don't want that, what if we want to exclude the inherited ones or some methods from a specific class?

```Java
void methodInfo(Object o) {
    Class<?> c = o.getClass();

    Method[] methods = c.getMethods();
    for(Method m: methods) {
        if(m.getDeclaringClass() != Object.class) { // Here
            System.out.println(m);
        }
    }

    Method[] declaredMethods = c.getDeclaredMethods();
    for(Method m: declaredMethods) {
        System.out.println(m);
    }
}
```

Also, we can access specific members by passing the member signature (name of field, or name of methods plus parameters, or parameter types of constructor) to the functions we used i.e. getFields, getMethods, etc.

Also, we can get the modifiers of members by using getModifier() with a certain member, then use Modifiers class to know what the modifiers of that member are.

### Interacting with Object Instances

So far we've accessed nearly everything about a Type.
But what about actually USING this info?

We can use reflection to perform Actions, we can invoke Members.
If we have a BankAccount object and a BankAccount reference pointing to it, we can invoke the Type's methods.
If we have a BankAccount object and an OBJECT reference pointing to it, we CAN NOT invoke the Type's methods that are not inherited from Object (methods declared inside BankAccount e.g. getId() or deposit()).
That's because the reference lacks the information about these methods.

Now, we can call getClass on the object and get the info we got in the last section.
So, we can get the _Class_ instance,
then get the info for getId(),
then apply it to the Object reference,
then use the Object reference now to call getId(), and the call would work :o

```Java
public static void main(String[] args) {
    BankAccount acct1 = new BankAccount("1");
    callGetId(acct1);
}

void callGetId(Object obj) {
    try {
        Class<?> c = obj.getClass();
        Method m = c.getMethod("getId");    // Now we have the info for getId
        Object result = m.invoke(obj);      // We invoked the method on the object reference of BankAccount acct1
    } catch(Exception e) {
        ...
    }
}

void callDeposit(Object obj, int amount) {
    try {
        Class<?> c = obj.getClass();
        Method m = c.getMethod("deposit", int.class);
        // deposit() has one parameter, an int, so we pass int.class, the type description of 'int'

        Object result = m.invoke(obj, amount); // We pass any arguments after the object reference
    } catch(Exception e) {
        ...
    }
}
```

While Reflection gives us runtime access to types and their members, it IS SLOWER Than compile-time access and usage.

### Creating instances of objects with Reflection

Since we can access a Type's constructors, we can invoke them and create new instances of a type.
If we're going to use the default constructor, we can use another method, newInstance(), which is a method of the class _Class_.

Why would we want that?
If we want to built a Work Dispatch system that:

- Executes worker classes against targets.
- Can use ANY WORKER in classpath.
- We just get passed the type NAMES.
- The method to start work has two args:
  - the worker type NAME, received as a string, and
  - the target of work, received as an object ref.
- Worker type requirements:
  - Should have a constructor that accepts target type.
  - A doWork method that takes no arguments.

```Java
// We'll use BankAccount and HighVolumeAccount
// We'll create a single worker, accountWorker
class AccountWorker implements Runnable {
    BankAccount ba;
    HighVolumeAccount hva;
    public AccountWorker(BankAccount ba) {...}
    public AccountWorker(HighVolumeAccount hva) {...}

    public void doWork() {
        Thread t = new Thread(hva != null ? hva : this);
        t.start();
    }

    public void run() {
        int amt = // read tx amount
        ba.deposit(amt);
    }
}

// So how to use Reflection to get all that working?

void startWork(String workerTypeName, Object workerTarget) {
    try {
        Class<?> workerType = Class.forName(workerTypeName);
        Class<?> targetType = workerTarget.getClass();

        Constructor c = workerType.getConstructor(targerType);
        Object worker = c.newInstance(workerTarget);

        Method doWork = workerType.getMethod("doWork");
        doWork.invoke(worker);
    } catch(Exception e) {
        ...
    }
}

public static void main(String[] args) {
    BankAccount acct1 = new BankAccount();
    startWork("com.pluralsight.tutorial.AccountWorker", acct1);
}
```

That's it, we can extend the app to include more workers and worker types.

#### Another way to create instances in reflection

Let's say we'll change the worker requirements:

- It should have a no-arg constructor.
- Implements TaskWorker.

```Java
public interface TaskWorker {
    void setTarget(Object target);
    void doWork();
}
```

Let's see how our code will change:

```Java
// We'll use BankAccount and HighVolumeAccount
class AccountWorker implements Runnable, TaskWorker {
    BankAccount ba;

    public AccountWorker() {...}
    public AccountWorker(BankAccount ba) {...}

    public void setTarget (Object target) {
        if(BankAccount.class.isInstance(target)) // if the target passed is an instance of BankAccount
        {
            ba = (BankAccount) target;
        } else {
            throw new IllegalArgumentException(...); // A good practice to raise exceptions
        }
    }

    public void doWork() {
        Thread t = new Thread(
            HighVolumeAccount.class.isInstance(ba) ? (HighVolumeAccount) ba : this // Notice the use of isInstance here
        );

        // In theory we could just pass ANY BankAccount child that uses Runnable

        t.start();
    }

    public void run() {
        int amt = // read tx amount
        ba.deposit(amt);
    }
}

// So how to use Reflection to get all that working?

void startWork(String workerTypeName, Object workerTarget) {
    try {
        Class<?> workerType = Class.forName(workerTypeName);
        TaskWorker worker = (TaskWorker) c.newInstance();   // Notice the difference here

        worker.setTarget(workerTarget);
        Worker.doWork();

        // Notice that when we casted the new instance to TaskWorker, we were able to just call its methods.
        // without invoke or getMethod, like normally.
        // that is a good practice too, just use reflection where you need it.

    } catch(Exception e) {
        ...
    }
}

public static void main(String[] args) {
    BankAccount acct1 = new BankAccount();
    startWork("com.pluralsight.tutorial.AccountWorker", acct1);
}
```

---

## Adding Type Metadata with Annotations

Why metadata and annotations?
As developers, when we create programs, usually they don't stand alone:

- They're hosted in an execution environment,
- that runs on some OS,
- and incorporate frameworks,
- or maybe running on a web environment,
- and our own assumptions.

When other developers work on the same code, they might have a hard time understanding our intent and context of each function, class, etc.

The type system, interfaces, modifiers, etc. all that helps clear this out, but not entirely.
For example, there is no difference in writing a function that is declared and writing a function that overrides another.

We might write comments or use documentation, and these are good but not enough.
We need a structured solution so that not only other developers but other TOOLS can understand and operate on the code correctly.

### Annotation

They are special Types that act as metadata.
They're applied to a specific target, but they don't change a target's behavior.
But they are interpreted by tools so that they understand the context and intent of our target.
We write an annotation just above the target.

Example:

```Java
class BankAccount {
    ...
    @Override // That's an annotation
    public String toString() {
        return String.format(getId() + ": " + getBalance());
    }
}
```

The compiler finds this annotation, then looks for the signature of the target in all superclasses,
if it finds one, that's great, if not, it generates an error.
So if we write toStrong instead of toString, without annotations we would know when we're finished and testing.
With annotations, we'll know at compile time.

Annotations are used by a lot of environment:

- Java EE.
- XML Processing tools like JAXP.
- etc.

Some annotations:

- Override.
- Deprecated: means that the target is old and not preferred to be used
- SuppressWarnings: the target generates warnings, but you know that's the case and you want to get rid of the warnings.

Let's look at an example:

```Java
class Doer {
    @Deprecated
    void doItThisWay() {...}
    void doItThisNewWay() {...}
}

// @SuppressWarnings("deprecation"): Don't do that
class MyWorker {
    @SuppressWarnings("deprecation")
    void doSomeWork() {
        Doer d = new Doer();
        d.doItThisWay();
    }

    @SuppressWarnings("deprecation")
    void doDoubleWork() {
        Doer d1 = new Doer();
        Doer d2 = new Doer();
        d1.doItThisWay();
        d2.doItThisWay();
    }
}
```

The developers of Doer write @Deprecated,
This generates warnings for anyone who uses the deprecated targets, notifying them that they should migrate to the new method.

Until the developers of MyWorker change the code and test it, they might use a @SuppressWarnings("deprecation") annotation to remove the warnings generated by the @Deprecated annotation.
This is dangerous to use on the class as it may supress other warnings.
So it's best used close to where the warnings come out, like on the specific functions.

### Custom Annotations

Annotations are a special type of interface:

- They can't be explicitly implemented.
- All annotations extend an interface: Annotation.
- But they don't feel like interfaces.

We can create custom annotations i.e. we can build solutions that operate based on custom metadata.
For example, remember this was used at the end of Reflections module:

```Java
class AccountWorker implements Runnable, TaskWorker {
    BankAccount ba;

    public AccountWorker() {...}
    public AccountWorker(BankAccount ba) {...}

    public void setTarget (Object target) {
        if(BankAccount.class.isInstance(target))
        {
            ba = (BankAccount) target;
        } else {
            throw new IllegalArgumentException(...);
        }
    }

    public void doWork() {
        Thread t = new Thread(
            HighVolumeAccount.class.isInstance(ba) ? (HighVolumeAccount) ba : this
        );

        t.start();
    }

    public void run() {
        int amt = // read tx amount
        ba.deposit(amt);
    }
}

void startWork(String workerTypeName, Object workerTarget) {
    try {
        Class<?> workerType = Class.forName(workerTypeName);
        TaskWorker worker = (TaskWorker) c.newInstance();

        worker.setTarget(workerTarget);
        Worker.doWork();

    } catch(Exception e) {
        ...
    }
}

public static void main(String[] args) {
    BankAccount acct1 = new BankAccount();
    startWork("com.pluralsight.tutorial.AccountWorker", acct1);
}
```

Now, we want to extend that system:

- Workers create their own threads.
- Can be run on an app's thread pool instead of their own threads.
- We'll use custom annotations to indicate whether they want their own threads, or the app's thread poo.

How to declare an annotation:

```java
public @interface WorkHandler {
    // Annotations can have content, called Elements.
    // Elements associate values within annotations.
    // They are declared as methods:
    boolean useThreadPool();
}
```

Alright, how can we use this annotation?

```Java
@WorkHandler(useThreadPool = false) //See here?
class AccountWorker implements Runnable, TaskWorker {
    ...
}
```

But we need to access the annotation's value so that we know if the worker wants their own thread or the app's thread pool.
How do we get info about annotations?

- We use Reflection:
  - target.getAnnotation():
    - We can pass type info of the annotation we're interested in.
    - returns an annotation's interface.
    - or null if the target isn't using annotation.

In our example, in the method startWork:

```Java
// First, in the dispatch program we're using, we'll declare a thread pool:
ExecutorService pool = Executors.newFixedThreadPool(5);
// Now, back to our method

void startWork(String workerTypeName, Object workerTarget) throws Exception {
    Class<?> workerType = Class.forName(workerTypeName);
    TaskWorker worker = (TaskWorker) c.newInstance();

    worker.setTarget(workerTarget);

    WorkHandler we = workerType.getAnnotation(WorkHandler.class);

    if(wh == null) {
        // Throw exception
    }

    if(wh.useThreadPool()) {
        // We'll submit an anonymous class of type Runnable to the program,
        // implement run(), then put worker.doWork() inside that.
        pool.submit(new Runnable() {
            public void run() {
                worker.doWork();
            }
        })
    } else {
        // Normal case, worker creates its own thread.
        Worker.doWork();
    }
}
```

See the exception that will be thrown if wh == null?
Actually, the exception in our case will be thrown, because the annotation is NOT available at runtime right now.

To solve this, we put an annotation Retention just before our annotation declaration.
Then pass it a RetentionPolicy value:

- SOURCE: make the annotation only available in the source file i.e. the annotation is discarded by the compiler, it never makes it into the class file.
- CLASS: compiled into class file, but discarded by runtime. (This is the default value)
- RUNTIME: now the annotation is loaded into the runtime, now it is accessible with reflection.

```Java
@Retention(RetentionPolicy.RUNTIME)
public @interface WorkHandler {
    boolean useThreadPool();
}
```

Now, the code in startWork method, will now work properly.

Now, the problem is, some people may misuse our annotation because:

- Annotations can be applied on anything: classes, methods, constructors, fields, and even local variables.
- We've done nothing to SPECIFY what our annotation can be applied to.

We solve this at the declaration of course.
We do that using another annotation, a Target annotation, which accepts ElementType, which has a long list of values we can use.
We also can use an array notation to specify multiple ElementType values, not just one.

```Java
@Target(ElementType.TYPE) //applies on Types only
@Retention(RetentionPolicy.RUNTIME)
public @interface WorkHandler {
    boolean useThreadPool();
}
```

Now, if someone misuses the annotation, the compiler will raise an error.

Note: the order of annotations applied on a target or an annotation declaration doesn't matter.

### A Closer Look At Elements

We can setup elements to simplify setting.

- Handling common cases.
- Default values for elements.

Let's say we want most workhandlers to use the thread pool in our example, so we give our element a default value

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface WorkHandler {
    boolean useThreadPool() default true;
    // See how we used default keyword
}
```

Sometimes we can assign an element a values without using the element's name, for that to happen:

- The annottion should have one element,
- whose name is 'value'

Look at this:

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface WorkHandler {
    boolean value() default true;
}

// Now we can use the annotation like this
@WorkHandler(false)
```

Elements are actually very restricted, there's only a very specific subset of Types that can be used as elements:

- Any primitive type.
- String.
- Enum.
- Annotation type.
- Class<?>: i.e. elements can store type information.
- Array whose type is any of the above.

## Serialization

We can persist objects i.e. take an object from runtime and store it as a byte stream.
We can know all the members of that object by reflection.

Serialization contains two things:

- Serializing: converting an object to a byte stream.
  Actually, when Java serializes an object, it serializes it AND all objects that are referenced by that object (Object graph).
- Deserializing: restoring an object and object graph from a byte stream.

Serializable Interface:

- implemented by any type that will be serialized.
- it has NO METHODS, it's a Marker interface (an interface to indicate something)

ObjectOutputStream: serializes object-graph to a stream.
ObjectInputStream: deserializes stream into object-graph.

### Being Serializable

We need to implement Serializable.
For a type to be serializable, all its members should be serializable:

- Primitives are serializable by default.
- Others must implement Serializable themselves.

Let's use the BankAccount example:

```Java
public class BankAccount {
    private final String id;
    private int balance = 0;

    // constructors

    // other methods
}
```

To make this serializable, make the members serializable:

- int balance is serializable.
- id: is a string, it's not a primitive, it's an object.
  i.e. values inside id are stored not inside BankAccount's memory, it's stored in a SEPARATE memory location, and id references it.
  This means that the BankAccount object is a very simple object-graph: an object that points to other objects.

  So, we need to make String serializable here.
  From the documentation we discover that String implements Serializable itself.

How to serialize an object

1. get a reference of the object to be serialized, and a destination and filename.
2. greate an ObjectOutputStream object with the path that is given.
3. using the ObjectOutputStream object, call the method writeObject and pass the object reference into it.

How to deserialize a stream:

1. create a reference of the same type as the object to be deserialized, pointing to null.
2. create an ObjectInputStream object, pass the stream file's path.
3. use the ObjectInputStream object, call the method readObject, it will return a reference to an object, receive it into the reference you created earlier.
4. cast that reference into the type you expect.
5. return the casted reference you just deserialized.

That's it ^\_^

Let's use an example:

```Java
// This is our driver code:

BankAccount acct = new BankAccount("1234", 500);
acct.deposit(250);
saveAccount(acct, "account.dat");

// Serializing
void saveAccount(BankAccount ba, String filename) {
    try(ObjectOutputStream objectStream = new ObjectOutputStream(Files.newOutputStream(Paths.get(filename))) {
        objectStream.writeObject(ba);
    } catch(IOException e) {
        // handle exception
    }
}

// Deserializing
BankAccount loadAccount(String filename) {
    BankAccount ba = null;
    try(ObjectInputStream objectStream = new ObjectInputStream(Files.newInputStream(Paths.get(filename)))) {
        ba = (BankAccount) objectStream.readObject();
    } catch(IOException e) {
        // handle exception
    } catch(ClassNotFoundException) {
        // Why?
        // because you can try to deserialize an object whose type is NOT in your class path.
        // in which case, this exception is thrown.
    }
    return ba;
}
```

### How changes to class definition affects serialization

Let's try to change BankAccount class:

```Java
public class BankAccount {
    private final String id;
    private int balance = 0;
    private char lastTxType;
    private int lastTxAmount;

    // constructors

    public synchronized void deposit(int amount) {
        balance += amount;
        lastTxType = 'd';
        lastTxAmount = amount;
    }

    public synchronized void withdraw(int amount) {
        balance -= amount;
        lastTxType = 'w';
        lastTxAmount = amount;
    }
}
```

If we try to deserialize the older object now (a BankAccount object but before the modifications we made), an InvalidClassException will be thrown.

How did Java know that?

- It calculates a Serial Version Unique Identifier: a hash value that identifies the structure of the class.
- This serial version unique identifier is included in the serializing process.
- When we tried to deserialize, Java calculates the serial version unique identifier of the Type definition in our app, and compare it with the one stored with the object stream.
- If they don't match up, an InvalidClassException is thrown, which means that both are not compatible anymore.

That serial version is calculated using:

- Full type name.
- Implemented interfaces.
- Members.
  The type content determines compatibility.

How do we solve this?
We can SPECIFY our serial version as part of our type definition.
i.e. we as developers get to determine the compatibility.

To do that, we need to:

- Add a field, a static final private long value called serialVersionUID
  private static final long serialVersionUID
- When we create the type, we calculate the initial value of the serial version.
  We do that using serialver utility,
- Then, we maintain this value, that's how we maintain compatibility.

The serialver utility:

- Performs the same calculation that Java does.
- Is found in JDK bin folder.
- A lot of IDEs provide a plug-in.

Using it:

- It uses the CLASS FILE to determine the serial version. So it will search in the classpath.
- Can pass class name to the command line.

Example:

```Java
package com.pluralsight.tutorial;

public class BankAccount implements Serializable {
    // all the stuff here
}
```

Using serialver:

```sh
$ serialver com.pluralsight.tutorial.BankAccount
# or we can use the -show option to use a very simple GUI
$ serialver -show
```

Adding the value to our type:

```Java
package com.pluralsight.tutorial;

// This is the old version
public class BankAccount implements Serializable {
    private static final long serialVersionUID = //write the value you get from serialver here.

    private final String id;
    private int balance = 0;

    // constructors

    // methods
}
```

Now, when we serialize this, Java will use Reflection to grab the value of that serialVersionUID field and write that into the stream, instead of calculating it again.

So, are they compatible now?

```Java
// This is the new version
public class BankAccount {
    private static final long serialVersionUID = //write the value you get from serialver here.

    private final String id;
    private int balance = 0;
    private char lastTxType;
    private int lastTxAmount;

    // constructors

    public synchronized void deposit(int amount) {
        balance += amount;
        lastTxType = 'd';
        lastTxAmount = amount;
    }

    public synchronized void withdraw(int amount) {
        balance -= amount;
        lastTxType = 'w';
        lastTxAmount = amount;
    }
}
```

Now, serialization before modification, then deserialization after modification, will work ;)

But, well, we may need to do more work to make them logically compatible, so what do we do?

### Custom Serialization

We can add custom serialization by adding writeObject and readObject methods to a type.
Both methods are called through reflection, so they are private.

How to implement writeObject:

- void return type.
- throws IOException.
- accepts ObjectOutputStream that's used to write values, and calls method defaulWriteObject() for default behavior.

How to implement readObject:

- void returntype.
- throws IOException and ClassNotFoundException
- accepts ObjectInputStream that's used to read values, call method readFields() to access values by the name of the field, and calls defaultReadObject() for default behavior.

```Java
class BankAccount implements Serializable {
    // all the stuff from before

    private void writeObject(ObjectOutputStream out) throws IOException {
        out.defaultWriteObject();
    }

    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        ObjectInputStream.GetField fields = in.readFields();
        //we have all fields here as a map or something

        id = (String) fields.get("id", null);
        // now we can get field values by name of the field
        // we cast it because it returns as Object

        balance = fields.get("balance", 0);
        // The last argument is a default value if no value for balance is found

        lastTxType = fields.get("lastTxType", "u");
        lastTxAmount = fields.get("lastTxAmount", -1);
    }
}
```

### Transient Fields

Sometimes, we don't want fields to be serialized, because:

- we calculate them from other fields, or
- avoid unnecessary memory usage.

We do that by using 'transient' keyword.

Example:

```Java
public class AccountGroup implements Serializable {
    private Map<String, BankAccount> accountMap = new HashMap<>();

    // Notice the use of transient here
    private transient int totalBalance;

    public int getTotalBalance() { return totalBalance; }
    public void addAccount(BankAccount account) {
        totalBalance += account.getBalance();
        accountMap.put(account.getId(), account);
    }
}
```

That class is serializable, all members are serializable.
The totalBalance can be calculated from the map, we don't need to serialize it.
so, we used the keyword transient in its declaration.

```Java
BankAccount acct1 = new BankAccount("1234", 500);
BankAccount acct2 = new BankAccount("5678", 700);

AccountGroup group = new AccountGroup();
group.add(acct1);
group.add(acct2);
saveGroup(group, "group.dat");
```

Serializing the AccountGroup would be the same as serializing BankAccount:

```Java
void saveGroup(AccountGroup g, String filename) {
    try(ObjectOutputStream objectStream = new ObjectOutputStream(Files.newOutputStream(Paths.get(filename))) {
        objectStream.writeObject(g);
    } catch(IOException e) {
        // handle exception
    }
}
```

Deserializing the AccountGroup:

```Java
AccountGroup loadGroup(String filename) {
    AccountGroup g = null;
    try(ObjectInputStream objectStream = new ObjectInputStream(Files.newInputStream(Paths.get(filename)))) {
        g = (AccountGroup) objectStream.readObject();
    } catch(IOException e) {
        // handle exception
    } catch(ClassNotFoundException) {
        // handle exception
    }
    return g;
}
```

But now, if we try to get the totalBalance, it will be 0 because we Excluded it from serialization by the keyword 'transient'.
So, we need to manually calculate it.

```Java
public class AccountGroup implements Serializable {
    // all the stuff from before

    void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        // Calling the default behavior because we won't modify it,
        // we'll extend it by calculating the transient values.
        in.defaultReadObject();

        // Here we do the calculation for totalBalance
        for(BankAccount a: accountMap.values()) {
            totalBalance += a.getBalance();
        }
    }

    // Notice we didn't implement writeObject, because we don't need to, we just
    // modify the readObject to calculate totalBalance because it's a transient field
}
```

And that is it everybody, thanks for reading, Java Core Platform, we're **DONEEEEEEEEEEEEEEEEE**

---