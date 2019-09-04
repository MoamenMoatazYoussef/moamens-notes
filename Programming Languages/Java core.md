# Java Core:

---

## Streams:

### Basucs

They are an ordered sequence of data, they are common io modules.
They are unidirectional i.e. we READ from, or we WRITE to, a single a stream.
Types:

- Byte streams: these are binary data.
- Text streams: these are unicode characters.

### Reader Class
This is the class we use to read bytes
- int read(): returns the value of bytethat was read, written as int.
- int read(byte[] buff): returns the number of bytes that were read in the buffer.

### InputStream Class
This is the class we use to read characters.
- int read(): returns the value of character that was read, written as int.
- int read(char[] buff): returns the number of characters that were read in the buffer.

Method we use with both classes:

How to read one byte:
``` Java
InputStream input = // create input stream.
int intVal;
while((intVal = input.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    byte byteVal = (byte) intVal;
    //do something with byteVal
}
``` 

How to read one character:
``` Java
Reader reader = // create input stream.
int intVal;
while((intVal = reader.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    char charVal = (char) intVal;
    //do something with charVal
}
``` 

How to read an array of bytes:
``` Java
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

Writing:
--------
### OutputStream
This is the class we use to write bytes.
- void write(int b): writes the byte 'b' to a source.
- void write(byte[] buff): writes list of bytes to a source.

### Writer
This is the class we use to write characters.
- void write(char ch): writes the byte 'c' to a source.
- void write(char[] buff): writes list of characters to a source.
- void write(String str): writes string to a source.

How to write bytes:
``` Java
OutputStream output = //create output stream
byte byteVal = 100;
output.write(byteVal);

//for many bytes
byte[] buff = {0, 63, 127};
output.write(buff)l
```

How to write characters or a string:
``` Java
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
Here's an example of using Java.nio Files and Paths classes instead of BufferedReader and FileReader

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

## Regular Expressions in Java

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

// stuff I didn't push from work

Collections use generics, which are specified during collection creation.
For example:
``` Java
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
``` Java
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
``` Java
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
``` Java
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
``` Java
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

``` Java
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
```

From Array to Collection: 
- Arrays.asList()

``` Java
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
``` Java
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
``` Java
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
``` Java
public class MyComparator implements Comparator<MyClass> {
    public int compare(MyClass x, MyClass y) {
        return x.getLabel().compareToIgnoreCase(y.getLabel()); 
        // Notice that we didn't have to use 'value' here
        // because we are using an external class to sort them
    }
}
```


How to sort a collection of MyClass using both methods:
``` Java
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
So we can really specify how things are sorited, which is a really useful thing.

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

``` Java
Map<String, String> map = new HashMap<>();
map.put("2", "c");
map.put("3", "a");
map.put("1", "b");
```

---

