# Basic I/O

## I/O Stream
An I/O Stream represents an input source or an output destination.


## Byte Streams
*  Perform input and output of 8-bit bytes
*  All byte stream classes are descended from `InputStream` and `OutputStream`.
*  **ALWAYS** close streams by invoking `close()` in finally block
*  **ALL** other stream types are built on byte streams.

### `FileInputStream` & `FileOutputStream`
*  Input and output int (holds a `byte` value) with `read()`.
*  Input and output byte array. 
*  Return -1 when reach the end of file.


## Character Streams
*  In addition to byte stream, character stream automatically translates to and from the local character set.
*  Java uses Unicode convention to store characters.
*  Usually, 8-bit superset of ASCII
*  All character stream classes are descended from `Reader` and `Writer`.

### `FileReader` & `FileWriter`
*  Input and output int  (hold a `character` value) with `read()`.
*  Input and output character array or String.
*  Return -1 when reach the end of file.


## Buffered Streams
*  Read data from buffer, then native input API is called only when the buffer is empty.
*  Write data to buffer, then native output API is called only when the buffer is full
*  Buffered stream classes:
  1. BufferedInputStream -> FileInputStream
  2. BufferedOutputStream -> FileOutputStream
  3. BufferedReader -> FileReader
  4. BufferedWriter -> FileWriter

  (created by wrapping unbuffered streams)

### Flushing Buffered Streams
Flushing the buffer: write out without waiting for it to fill.

`flush()` is valid on any output stream, but has no effect unless the stream is buffered.


## Scanner API
*  Formatted input -> tokens -> corresponding data type
*  Default delimiter white space including blanks, tabs, and line terminators. 
*  Set delimiter: 'useDelimiter(regex)'.
*  `hasNext()` checks EOF
*  `next()` returns String
*  Set locale: `useLocale(Locale.<location>)`.


## Formatting
Stream objects that implement formatting are instances of either `PrintWriter` (character stream) or `PrintStream` (byte stream).

**note:** The only `PrintStream` objects we are likely to need are `System.out` and `System.err`.

*  `print()` & `println()`: standard way
*  `format()`: based on a format string.


## Standard Streams
1. Standard Input: `System.in`
2. Standard Error: `System.err`
3. Standard Output: `System.out`


*  All standard streams are byte streams.
*  `System.out` & `System.err` -> `PrintStream` which utilizes an internal character stream object to emulate many of the features of character streams.
*  `System.in` has no character stream feature. But we can wrap it in `InputStreamReader`.


## The Console
*  Useful for secure password entry.
*  Provides both input and output streams that are true character streams.
*  Invoking `System.console()` to retrieve `Console` object if it is available.
*  `readPassword()` returns char array.


## Data Streams
*  Support binary I/O of primitive data type values as well as String values.
*  `DataInputStream` -> `DataInput`.
*  `DataOutputStream` -> `DataOutput`.
*  ReturnType read\[Type\]/write\[Type\] ().
*  Throws `EOFException` when reach of EOF.
*  Don't use floating point numbers to represent monetary values because of bad decimal fractions. We should use `Object Stream` & `BigDecimal`.


## Object Streams
* Support I/O of objects.
* `ObjectInputStream` -> `ObjectInput` -> `DataInput`
* `ObjectOutputStream` -> `ObjectOutput` -> `DataOutput`
* complex object writes all its referenced objects to stream.
* Two objects -> single object on the same stream => Two objects -> single object when read back.


# File I/O
## Path Class
* reflects underlying platform.
* creation: 
  1. `Paths.get(<path>)` <=> `FileSystems.getDefault().getPath(<path>)`.
  2. `Paths.get(<dir1>, <dir2>, <fileName>)`
* Path stores name elements as a sequence: `/home/user/foo` => `[home, user, foo]`.
  1. `getName(index)`
  2. `getNameCount`: number of elements in the path
  3. `subpath(beginIndex, endIndex)`
  4. `getParent()`: elements[0:n-2]
  5. `getRoot()`: if it is relative path, it returns null.
* Converting a Path:
  1. `toUri()`
  2. `toAbsolutePath()`
  3. `toRealPath()`
    1. if true is passed to this method and the file system supports symbolic links => resolves symbolic links.
    2. relative path => absolute path
    3. remove redundant elements.
* Joining Two Paths: `resolve(<relativePath>)`.
* Creating a path between two paths: `p1.relativize(p2)`.
* Comparing two paths:
  1. `equals()`
  2. `startsWith(<path>)`
  3. `endsWith(<path>)`


## Checking a File or Directory
* Checking existence:
  1. verified to exist: `exists(Path, LinkOption...)`
  2. verified to not exist: `notExists(Path, LinkOption...)`
  3. status is unknown.
* Checking Accessibility:
  1. `isReadable(Path)`
  2. `isWritable(Path)`
  3. `isExecutable(Path)`
* Checking whether two paths locate the same file.


## Deleting a File or Directory
* If deleting symbolic link, only the link is deleted.
* `delete(Path)`: throws an exception if the deletion fails.
* `deleteIfExists(Path)`: no exception is thrown.


## Copying a File or Directory
`copy(sourcePath, targetPath, CopyOptions...)`

* `copy(InputStream, Path, CopyOptions...)`: copy all bytes from an input stream to a file.
* `copy(Path, OutputStream)`: copy all bytes from a file to an output stream.


## Moving a File or Directory
`move(sourcePath, targetPath, CopyOptions...)`


## Managing Metadata




