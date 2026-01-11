
---

# **Comprehensive Notes: Java I/O, NIO, and NIO.2**

## **1. Conceptual Overview & Analogies**
To understand the ecosystem, visualize a **Water Bottling Plant**:
*   **Data:** Water.
*   **Source/Destination:** Lake (Source File) / Bottle (Destination File).
*   **Stream (java.io):** A narrow garden hose. Data flows in **one direction** (read OR write), drop-by-drop (byte-by-byte). It is blocking.
*   **Channel (java.nio):** A multi-lane highway or railway. Data flows **bi-directionally** (read AND write). High capacity, often non-blocking.
*   **Buffer (java.nio):** A bucket/container. Instead of moving single drops, you fill the bucket (buffer) and move the whole bucket at once.
*   **Selector (java.nio):** A receptionist. One thread can monitor multiple channels (phone lines) and handle only the ones that are "ringing" (ready for I/O).

---

## **2. Locating Resources: File vs. Path**

### **Legacy: `java.io.File` (Java 1.0)**
*   **Functionality:** Represents a path to a file or directory. It is an abstract representation; creating the object does not create the file on disk.
*   **Drawbacks:**
    *   Many methods return `boolean` (false) on failure instead of throwing exceptions, making debugging difficult.
    *   `renameTo` is inconsistent across platforms.
    *   Limited metadata support (permissions, owners).
    *   Not scalable for listing large directories (can cause hangs or OOM errors).

### **Modern: `java.nio.file.Path` (Java 7 / NIO.2)**
*   **Functionality:** The modern replacement for `File`. It is an interface representing a hierarchical sequence of directory and file names.
*   **Key Features:**
    *   **OS Independent:** Handles system-dependent syntax (slashes vs. backslashes).
    *   **Factory Methods:** Created via `Paths.get()` (Java 7) or `Path.of()` (Java 11).
    *   **Interoperability:** Can convert to/from `File` using `toPath()` and `toFile()`.
*   **Key Components:**
    *   **FileSystem:** Factory for objects to access files/objects. Obtained via `FileSystems.getDefault()`.
    *   **FileStore:** Represents the underlying storage device (partition/volume). Used to check space (`getTotalSpace`).

---

## **3. The `java.io` Package (Streams)**

### **Byte Streams (Raw Data)**
Used for images, audio, video, and raw binary.
*   **`InputStream` / `OutputStream`:** Abstract base classes. Read/write 8-bit bytes.
*   **`FileInputStream` / `FileOutputStream`:** Connects streams to files. Reading returns `-1` at EOF.
    *   *Note:* Using these byte-by-byte is inefficient; they should be buffered.

### **Character Streams (Text Data)**
Used for text files (`.txt`, `.json`, `.java`). Handles Unicode/Encoding automatically (16-bit).
*   **`Reader` / `Writer`:** Abstract base classes for character I/O.
*   **`FileReader` / `FileWriter`:** Convenience classes for reading/writing text files using the default or specified encoding.
    *   *Warning:* In older Java versions, `FileReader` used the default system encoding which could cause portability issues. Java 11 added constructors to specify Charset.

### **Buffering (Decorators)**
Wraps streams to reduce system calls (context switches) by processing chunks of data.
*   **`BufferedReader`:** Reads text efficiently. Key method: `readLine()`.
*   **`BufferedWriter`:** Writes text efficiently. Key method: `newLine()`.
*   **`BufferedInputStream` / `BufferedOutputStream`:** The byte-stream equivalents.

### **Bridges**
*   **`InputStreamReader`:** Converts Byte Streams (`InputStream`) → Character Streams (`Reader`). It decodes bytes to characters using a Charset.
*   **`OutputStreamWriter`:** Converts Character Streams (`Writer`) → Byte Streams (`OutputStream`). It encodes characters into bytes.
    *   *Clarification:* There is no `BufferedInputStreamReader`. You wrap an `InputStream` in an `InputStreamReader`, and then wrap *that* in a `BufferedReader`.

---

## **4. The `java.nio` Package (New I/O - Java 1.4)**

### **Core Components**
*   **Channel:** An open connection to hardware (File, Socket). Can be non-blocking. Implementations include `FileChannel`, `SocketChannel`.
*   **Buffer:** A block of memory (`ByteBuffer`, `CharBuffer`) used to hold data. You read from a Channel *into* a Buffer, or write from a Buffer *to* a Channel.
    *   Key methods: `flip()` (switch from writing to buffer to reading from buffer), `clear()`, `compact()`.
*   **Selector:** Monitors multiple channels for events (connection open, data ready). Allows a single thread to manage thousands of connections (multiplexing).

### **FileChannel**
*   Supports advanced features like **Memory-Mapped Files** (`map()`) for handling very large files efficiently by mapping them directly into memory.
*   Supports file locking (`lock()`) to prevent access by other programs.

---

## **5. The `java.nio.file` Package (NIO.2 - Java 7)**

### **The `Files` Utility Class**
Static methods that operate on `Path` objects.
*   **CRUD:** `createFile`, `createDirectory`, `delete`, `deleteIfExists`, `copy`, `move`.
*   **Checks:** `exists`, `notExists`, `isReadable`, `isWritable`, `isDirectory`.
*   **Metadata:** `readAttributes` (bulk read of size, creation time, etc.).
*   **Listing:** `newDirectoryStream` (lazy iteration of directory contents).

### **Advanced Operations**
*   **`FileVisitor`:** Interface for recursively walking file trees (e.g., recursive delete or copy). Used with `Files.walkFileTree`.
*   **`WatchService`:** API to monitor directories for events like `ENTRY_CREATE`, `ENTRY_MODIFY`, `ENTRY_DELETE`. Useful for hot-reloading configurations.
*   **Symlinks:** `Files.createSymbolicLink`, `Files.isSymbolicLink`. `Path` methods are link-aware.

---

## **6. Code Examples: Classic vs. Modern**

### **Topic 1: Reading a Text File Line-by-Line**

**Classic Way (Java 1.1 - 6)**
*Uses `BufferedReader` and `FileReader` explicitly. Note the nested closing complexity or lack of auto-close.*
```java
// Java 1.1+ (Manual Close)
import java.io.*;

public class ClassicRead {
    public static void main(String[] args) {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader("data.txt")); //
            String line;
            while ((line = reader.readLine()) != null) { // Returns null at EOF
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (reader != null) reader.close(); //
            } catch (IOException e) { e.printStackTrace(); }
        }
    }
}
```

**Modern Way (Java 7+ with NIO.2)**
*Uses `try-with-resources` (Java 7), `Path` (Java 7), and `Files.newBufferedReader`.*
```java
// Java 7+ (Try-with-resources)
import java.nio.file.*;
import java.io.*;

public class ModernRead {
    public static void main(String[] args) {
        Path path = Paths.get("data.txt"); //
        // Automatic resource closing
        try (BufferedReader reader = Files.newBufferedReader(path)) { //
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Ultra-Modern Way (Java 8+ Streams & Java 11 String Methods)**
```java
// Java 8 (Streams)
try (Stream<String> lines = Files.lines(Paths.get("data.txt"))) { //
    lines.forEach(System.out::println);
}

// Java 11 (Read whole string)
String content = Files.readString(Path.of("data.txt")); //
System.out.println(content);
```

---

### **Topic 2: Writing to a Text File**

**Classic Way**
```java
// Java 1.1+
import java.io.*;

public class ClassicWrite {
    public static void main(String[] args) {
        try {
            // boolean true for append mode
            BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt", true)); 
            writer.write("Hello World");
            writer.newLine(); //
            writer.close();
        } catch (IOException e) { e.printStackTrace(); }
    }
}
```

**Modern Way (Java 7/11)**
```java
// Java 7+
import java.nio.file.*;
import java.util.Arrays;

public class ModernWrite {
    public static void main(String[] args) {
        Path path = Path.of("output.txt"); // Java 11 Factory
        String content = "Hello NIO World";
        try {
            // Writes bytes or lines in one go
            Files.write(path, content.getBytes(), StandardOpenOption.CREATE, StandardOpenOption.APPEND);
            
            // Java 11 specific string write
            Files.writeString(path, "\nJava 11 is cool", StandardOpenOption.APPEND); 
        } catch (IOException e) { e.printStackTrace(); }
    }
}
```

---

### **Topic 3: Directory Listing & Walking**

**Classic Way (File Array)**
*Problems: Returns null if path isn't a directory, reads everything into memory (OOM risk).*
```java
// Java 1.0
File dir = new File("myDir");
File[] files = dir.listFiles(); //
if (files != null) {
    for (File f : files) {
        System.out.println(f.getName());
    }
}
```

**Modern Way (DirectoryStream - Java 7)**
*Lazy loading, memory efficient.*
```java
// Java 7
Path dir = Paths.get("myDir");
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) { //
    for (Path entry : stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException e) { e.printStackTrace(); }
```

**Modern Way (Walk File Tree - Java 7)**
*Recursively visiting files.*
```java
// Java 7
Files.walkFileTree(dir, new SimpleFileVisitor<Path>() { //
    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) {
        System.out.println("Visited: " + file);
        return FileVisitResult.CONTINUE;
    }
});
```

**Modern Way (Java 8 Streams)**
```java
// Java 8
try (Stream<Path> stream = Files.walk(Paths.get("myDir"))) { //
    stream.filter(Files::isRegularFile)
          .forEach(System.out::println);
}
```

---

### **Topic 4: Copying with Streams (Binary)**

**Classic Way (Manual Buffer)**
```java
// Java 1.0+
try (FileInputStream in = new FileInputStream("source.jpg");
     FileOutputStream out = new FileOutputStream("dest.jpg")) {
    
    byte[] buffer = new byte; //
    int length;
    while ((length = in.read(buffer)) > 0) {
        out.write(buffer, 0, length);
    }
}
```

**Modern Way (Java 7 Files Copy)**
```java
// Java 7
Path source = Paths.get("source.jpg");
Path dest = Paths.get("dest.jpg");
Files.copy(source, dest, StandardCopyOption.REPLACE_EXISTING); //
```

**Modern Way (Java 9 Transfer)**
```java
// Java 9
try (InputStream in = new FileInputStream("source.jpg");
     OutputStream out = new FileOutputStream("dest.jpg")) {
    in.transferTo(out); //
}
```

---

## **7. Summary of Key Classes by Category**

| Category | Key Interfaces/Classes | Description | Version |
| :--- | :--- | :--- | :--- |
| **Pathing** | `Path`, `Paths` | Replaces `File`. Use `Path.of` or `Paths.get`. | Java 7/11 |
| **Operations** | `Files` | Static utility for CRUD, copy, move, attributes. | Java 7 |
| **Byte I/O** | `InputStream`, `OutputStream` | Abstract bases for raw binary data. | Java 1.0 |
| **Char I/O** | `Reader`, `Writer` | Abstract bases for text. Handles encoding. | Java 1.1 |
| **File I/O** | `FileInputStream`, `FileReader` | Implementation for files. `FileReader` now accepts Charset. | Java 1.0/11 |
| **Buffered** | `BufferedReader`, `BufferedInputStream` | Increases performance by reading chunks. | Java 1.1 |
| **Conversion** | `InputStreamReader` | Bridge from Byte Stream to Char Stream. | Java 1.1 |
| **NIO Core** | `Channel`, `Buffer`, `Selector` | Non-blocking, high-performance, multiplexed I/O. | Java 1.4 |
| **Monitoring** | `WatchService` | Receive notifications on file modification/creation. | Java 7 |
| **Traversing** | `FileVisitor`, `SimpleFileVisitor` | Recursively walk directory trees safely. | Java 7 |

## **8. Important Interview Concepts**
*   **Virtual Threads (Java 21):** While NIO is non-blocking, Project Loom (Virtual Threads) allows writing simple blocking IO code (like `java.io`) that scales like NIO because the JVM handles the blocking cheaply.
*   **Try-with-resources:** Always use this. It handles closing streams automatically (implements `AutoCloseable`), preventing memory leaks.
*   **Decorator Pattern:** Java I/O heavily uses this (e.g., `new BufferedReader(new InputStreamReader(new FileInputStream(...)))`).


---

> For more detailed references : 

[Java I/O and NIO - Baledung Blog Lists](https://www.baeldung.com/java-io)

[Java Java I/O and NIO - Medium Blog](https://medium.com/javarevisited/mastering-java-file-handling-from-java-io-streams-to-nio-in-2025-1050c3e7854a)

[Java I/O - Jenkov Blog](https://jenkov.com/tutorials/java-io/index.html)

[Java NIO - Jenkov Blog](https://jenkov.com/tutorials/java-io/index.html)
