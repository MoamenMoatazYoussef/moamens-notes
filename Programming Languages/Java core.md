# Java Core:

---

## Streams:

### Basucs

They are an ordered sequence of data, they are common io modules.
They are unidirectional i.e. we READ from, or we WRITE to, a single a stream.
Types:

- Byte streams: these are binary data.
- Text streams: these are unicode characters.

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

So, a type implementing the AutoCloseable interface, supports a stable structure in java called Try-with-resources.

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

The java.io packages have streams for files:

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
#### java.nio Classes For File Streams
Since java.io file stream classes are deprecated, there are new ones in the java.nio package.
These provide many benefits over java.io:
- Better exception reporting.
- They deal with large files more efficiently.
- They support more features of the file system.
- Simplify a lot of common tasks.

#### Path and Paths classes
These are classes used to locate files and reference them:
``` Java
Path p1 = Paths.get("path/to/file.txt");
Path p2 = Paths.get("path", "to", "file.txt");

// Both p1 and p2 point to the same file.
```

The java.nio package provide Files and Paths classes which provide a lot of methods to simplify streams:
They contain factory methods that handle the creation of FileReaders, writers, etc.
Here's an example of using java.nio Files and Paths classes instead of BufferedReader and FileReader
``` Java
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
``` Java
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
``` Java
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

``` Java
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
```
That is awesome xD

