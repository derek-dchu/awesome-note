# Security

### What is the preferred data type to store sensitive data (password) in Java.

Char array. We use `char[]` instead of `String`, because String is immutable, we create a String as the password, it will store in memory until garbage collector collects it, in other words, a malicious program can search around the memory and sense this data. For char[], we don't need to wait for garbage collector, instead, we can explicitly wipe the data by writing irrelevant data into it. There is why `char[]` is more secure than `String` for storing sensitive data.
