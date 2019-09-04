Java Core:
==========

-------------------------------------------------------------------------------------------------------------------------------------------------
Streams:
--------
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
``` java
InputStream input = // create input stream.
int intVal;
while((intVal = input.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    byte byteVal = (byte) intVal;
    //do something with byteVal
}
``` 

How to read one character:
``` java
Reader reader = // create input stream.
int intVal;
while((intVal = reader.read()) >= 0) { //as long as the value is 0 or greater, we got a valid byte.
    char charVal = (char) intVal;
    //do something with charVal
}
``` 

How to read an array of bytes:
``` java
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
``` java
OutputStream output = //create output stream
byte byteVal = 100;
output.write(byteVal);

//for many bytes
byte[] buff = {0, 63, 127};
output.write(buff)l
```

How to write characters or a string:
``` java
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

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Collections
