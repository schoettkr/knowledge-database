+++
title = "Algos & Programming - Lecture 18"
author = ["eo shiru"]
date = 2018-12-07
lastmod = 2019-04-06T00:17:37+02:00
draft = false
+++

## Text Search {#text-search}


### Files {#files}

Bigger volumes of data are usually not entered interactively but rather stored in **files**. That's why we take a look at files first: A file is a set of data that logically belongs together and is treated as a unit. Files are usually acessed by a _file name_ that is known to the operating system and get stored on persistant data volumes (eg harddrives).

Files can be organized differently. In the sense of the UNIX philosophy a file is a single set of bytes with an arbitrary size. Such data set resp sequence of bit is also called **bitstream** resp. **bytestream**.

The input/ouput of the C standard library is adapted to this concept so that there is no differentitation required between input from an input device or a file. To be more precise, the C standard library knows two types of file operations:

-   **low level file operations**
    -   files and file/data streams are identified via a **handle**
    -   specific to the particular operating system
-   **high level file operations**
    -   files and file/data streams are identified via a **file pointer**
    -   independant of the operating system

For now we'll look at **high level file operations**.

Files have to be **opened**. When opening a file the management/administration information is created (Verwaltungsinformationen). There's a function in the standard library to open files `FILE* fopen(char* name, char* mode)` which returns a pointer to the file management structure (Dateiverwaltungsstruktur) or NULL:

```C
#include <stdio.h>

/* ... */

FILE *fp;

fp = fopen("myFile.dat", "r");
```

A file that is no longer needed should be closed and the management resources shoudl eb released. That is done via `int fclose(FILE* stream)` which returns `0` when the file was closed successfully.

Here's a list of the possible file access modes which are passed to `fopen`:

-   `"r"` &rarr; **read**: open file for input operations (reading from the file); the file must exist
-   `"w"` &rarr; **write**: create an empty file for output operations (writing to a file); if a file with the same name already exists, its contents are discarded and the file is treated as a new empty file
-   `"a"` &rarr; **append**: open file for output at the end of a file; output operations always write data at the end of the file, thus expanding it; repositioning operations (fseek, fsetpos, rewind) are ignored; the file is created if it does not exist
-   `"r+"` &rarr; **read/update**: open a file for update (both for input and output); the file must exist
-   `"w+"` &rarr; **write/update**: create an empty file and open it for update (both for input and output); if a file with the same name already exists its contents are discarded and the file is treated as a new empty file
-   `"a+"` &rarr; **append/update**: open a file for update (both for input and output) with all output operations writing data at the end of the file; repositioning operations (fseek, fsetpos, rewind) affects the next input operations, but output operations move the position back to the end of file; the file is created if it does not exist

With the _mode specifiers_ above files are opened as _text files_. In order to open a file as a _binary file_ a `"b"` character has to be included in the mode string.  This additional "b" character can either be appended at the end of the string (thus making the following compound modes: "rb", "wb", "ab", "r+b", "w+b", "a+b") or be inserted between the letter and the plus sign for the mixed modes ("rb+", "wb+", "ab+").

For data input and output, C provides a collection of library functions. These functions enable the transfer of data between the C program and standard input/output devices. C always treats all input-output data, regardless of where they originate or where they go, as a stream of characters.
The operating system makes the input and output devices available to a C program as if these devices were files. So, essentially, when a C program reads data from the keyboard, it is in effect reading from the file associated with the keyboard device. When a C program sends output data to the console, it is in effect writing to the file associated with the console device.

A stream of characters or text stream, is a sequence of characters divided into lines. Each line consists of various characters followed by a newline character (\n). All input-output functions in C conform to this model.

In order to be able to use the above mentioned input-output functions in your C program, you must begin each C program with a pre-processor directive to include these standard library functions.

This can be done via `#include <stdio.h>`.

These are the most common/essential input-output functions:

|            | Input                                        | Output                                        |
|------------|----------------------------------------------|-----------------------------------------------|
| formatted  | `int fscanf(FILE*, char*, ...)`              | `int fprintf(FILE*, char*, ...)`              |
| characters | `int fgetc(FILE*)`                           | `int fputc(int, FILE*)`                       |
| strings    | `char* fgets(char*, int, FILE*)`             | `int fputs(char*, FILE*)`                     |
| binary     | `size_t fread(void*, size_t, size_t, FILE*)` | `size_t fwrite(void*, size_t, size_t, FILE*)` |
|            |                                              |                                               |

-    Formatted data input & output

    `fscanf` and `fprintf` work like `scanf` and `printf` but take a file pointer as an additional first parameter. To use `fscanf` the file has (at least) to be opened in read mode ("r", "r+", "w+", "a+"). To use `fprintf` the file has (at least) to be opened in write mode ("w", "a", "r+", "w+", "a+").

    ```C
    #include <stdio.h>

    int main ()
    {
      FILE* file;
      int n = 42;
      file = fopen("out.txt", "w");
      fprintf(file, "Hello world! The answer is %d\n", n);
      fclose(file);
      return 0;
    }
    ```

-    Characterwise data input & output

    The function `int fgetc(FILE* stream)` returns the next character of a file `stream` as an integer. When there's no character left, the constant `EOF` (defined in `stdio.h`) is returned (same thing when an error occurs).

    The function `int fputc(int c, FILE* stream)` writes the integer coded character `c` into the file `stream` and returns the number of written characters (= 1). In case of an error it returns `EOF`.

-    String data input & output

    The function `char* fgets(char restrict * str, int n, FILE* restrict stream)` reads maximally `n-1` characters from the file `stream` into a character string that is pointed to by `str`. The reading proccess ends with the end of the line/file or when an error occurs. When no error occurs `\0` is appended to `str` and the return value points to `str` (and to `NULL` if there was an error). Beware that it is the duty of the programmer to guarantee that `str` points to an character array which has a size of at least `n` characters.

    The function `int fputs(char* str, FILE* stream)` writes the (zero-terminated) string `str` into the file `stream`. It returns a non-negative integer on success and `EOF` in case of an error (old C versions used to return `0` on success)

-    Binary data input & output

    The function `size_t fread(void* ptr, size_t size, size_t nitems, FILE* stream)` reads `nitems` of size `size` from the file `stream` and stores them at the address specified by `ptr`. It then returns the count of successfully read items/elements (not bytes!).

    The function `size_t fwrite(void* ptr, size_t size, size_t nitems, FILE* stream)` writes `nitems` from the address `ptr` of size `size` in the file `stream` and also returns the count of successfully written elements/items (not bytes).

-    Standard Data Streams

    The standard input-output devices or the associated files or text streams, are referred to as:

    -   **stdin** - standard input file, normally connected to the keyboard
    -   **stdout** - standard output file, normally connected to the screen/console
    -   **stderr** - standard error display device file, normally connected to the screen/console

    `stdin`, `stdout`, `stderr` don't need to be opened like other files/streams (and cannot be opened):

    ```C
    #include <stdio.h>

    int main() {
      fprintf(stdout, "This is usage data.\n");
      fprintf(stderr, "This is status data.\n");
      return 0;
    }
    ```


#### Manipulating the File Position Pointer {#manipulating-the-file-position-pointer}

See: <https://stackoverflow.com/questions/39687795/what-is-file-position-pointer>

Usually files are treated as data stream, which are accessed **sequentally**. In case of "real files" it is possible to deviate from this sequential access. The following functions may be used to do so:

-   `void rewind(FILE* stream)` &rarr; move the read or write position in the file `stream` back to the beginning of the file
-   `void fseek(FILE* stream, long offset, int whence)` &rarr; moves the read or write position in the file `stream` to a position which is `offset` bytes shifted from `whence` (von wo/woher)
    -   `whence` shall be one of the following constants which are defined in `stdio.h`
        -   `SEEK_SET` = offset relative to the beginning of the file
        -   `SEEK_CUR` = offset relative to the current position in the file
        -   `SEEK_END` = offset relative to the end of the file
-   `long ftell(FILE* stream)` &rarr; may be used to get the current position in the file relative to the beginning of the file

Slides 17-19 provide code examples for reading/writing a file.

A few other interesting functions in regards to files are:

-   `int feof(FILE* stream)` returns a value &ne; 0 when at the end of the file
-   `int ferror(FILE* stream)` returns a value &ne; 0 when an file error has occured before
-   `int flush(FILE* stream)` forces a physical write (emptying the cache)
-   `int remove(char* name)` deletes the file with a name of `name`


#### Files in Python {#files-in-python}

F21
A file is a data type in Python. A data variable is created via `f = open(filename[, mode[, bufsize]])` .
The possible modes are a superset of the modes we know from C and with `bufsize` the cache size for the file can be set.
This is the Python 3 [documentation](https://docs.python.org/3/library/functions.html#open) for `open` (which looks kinda different than the slides).

Here are a few common file operations in Python (`file` be a data variable):

-   `S = file.read()` reads the whole file into a single string
-   `S = file.read(N)` reads `N` bytes
-   `S = file.readline()` reads the next line (until new line char)
-   `L = file.readlines()` reads the whole file as a list of line strings
-   `file.write(S)` writes the string `S` into the file
-   `file.writeLines(L)` writes all strings in a list `L` into the file
-   `file.close()` closes the file

Using iterations it is easy to work with a whole file in Python:

```python
f = open("foo.txt", "r")
for line in f:
    print(line, end = ' ')
```

And Python provides more modules for file manipulations

-   module `os` for low level
-   module `shelve` and `pickle` for high level storage of complex objects
-   module `dbm` and `anydbm` for database interfaces


### Simple Search {#simple-search}

Now with our newly acquired knowledge about files we can start looking into text search.

Our program should take the following parameters:
`./search <searchText> <fileName>`

And if the search text is found in the file, then the "surrounding" in which it was found should be returned, while the search text is wrapped in brackets to accentuate, eg

```sh
./search "example" lorem.txt

ullamcoprer subsciptit nisl ut aliqup [example] ea commodano
```

One of the first problems we encounter is that we don't know the size/length of neither a line nor the whole file. Here a few solution approaches:

-   Approach 1: define a line buffer that is "sufficiently large" for all cases &rarr; not safe and not a good approach in general
-   Approach 2: don't always read in whole lines &rarr; complicates the search if the search text is between two read-in blocks
-   Approach 3: determine the file size, dynamically reserve space and read in the whole file &rarr; requires a lot of memory storage

We go with approach 3 since it also offers speed advantages.

So let's determine the file size first - how do we do that?
The unix C function `int stat(char* name, struct stat* buf)` which writes informations about the file `name` into `buf` is not compatible so we don't use it and instead rely on using a combination of functions from the standard library:

```C
size_t filesize(FILE* file) {
  size_t ret;
  fseek(file, 0L, SEEK_END); // offset the file position pointer by 0 bytes relative to the eof
  ret = ftell(file); // get the current position relative to the beginning of the file
  rewind(file); // move the file position pointer back to the beginning of the file
  return ret;
}
```

By the way `size_t` is an OS dependant unsigned integer type that can store the maximum _size_ of a theoretically possible object of any type (including array) and which is commonly used for array indexing and loop counting (Programs that use other types, such as `unsigned int`, for array indexing may fail on, e.g. 64-bit systems when the index exceeds `UINT_MAX` or if it relies on 32-bit modular arithmetic.).

And this will be our main function which uses our `filesize` function amongst other things:

```C
#include <stdio.h>

int main(int argc, char* argv[]) {
  FILE* file;
  char* text;

  if (argc != 3) return -2; // wrong number of params

  if (file = fopen(argv[2], "r") == NULL) { // open file
    return -3; // cant open file
  }

  size_t size = filesize(file);

  /* allocate size+1 (for terminating 0) memory for our file buffer */
  if ((text = malloc(size+1)) == NULL) {
    return -4; // out of memory
  }

  /* read one element of size 'size' into our text buffer (the whole file) */
  if (fread(text, size, 1, file) != 1) {
    return -5; // can't read file
  }

  text[size] = '\0'; // set the terminating 0;

  int found = search(argv[1], text, size); // yet to implement!
  if (found != -1) {
    presentResult(found, text, argv[1]); // dito!
  }

  free(text);

  return found;
}
```

This main function does the necessary preparations for the actual search. Besides the search function we also need a function for the presentation/output. We want to output 20 characters before and after the search string:

```C
void presentResult(int pos, const char* str, const char* pattern) { // pos is the start of match position, str is the file buffer and pattern our search text
  int start, end, patlen, prelen;

  start = pos > 20 ? pos - 20 : 0; // output beginning

  prelen = pos > 20 ? 20 : pos; // beginning of match
  patternLength = length(pattern); // yet to implement!

  end = pos + patternLength;

  printf("%.*s[%s]%.20s\n", prelen, &str[start], pattern, &str[end]);
}
```

As seen in the code listing above we also need a function to determine the length of a string. There's an function for that in the standard library but we'll use our own:

```C
int length(const char* str) {
  int len = 0;
  if (str == NULL) return 0;

  while (str[len] != '\0') {
    ++len;
  }

  return len;
}
```

Now we can finally turn our attention to the actual search algorithm. The idea is that we want to test for each position in the text `str`, if the searchstring `p` begins there. If that is the case, then we want to test the next character and so on..

Here's the pseudocode:

```C
// str is file/text buffer and p is search string
Require: str and p is text, length(str) > length(p)
Ensure: returns index of first appearance of p in str

procedure SIMPLE-SEARCH(str, p)
  pos = 1
  while pos < length(str) - length(p) do
    j = 1
    while ((j <= length(p)) and (str[pos+j-1]) = p[j]) do
      if j = length(p) then // found 1st occ of search string
        return pos
      endif
      j = j+1
    end while
    pos = pos + 1
  end while
  return "not found"
end procedure
```

The actual C implemenation is where we'll continue in the next lecture (19), have a nice day (◕‿‿◕)
