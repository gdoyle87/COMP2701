# Week 11

This week continued where week 10 left off and continues looking at 
helpful string functions in different libraries.

## Input/Output Functions (`<stdio.h>`)

--- 

#### `getchar(void)`
Returns the next character from standard input as an `int`.

---

#### `putchar(int c)`
Prints the character represented by `c`.

---

#### `puts(const char *s)`
Prints the string `s` followed by a newline.

---

#### `sprintf(char *s, const char *format, ...)`
Exactly like `printf` but instead of printing to stdout it *prints* to
a string variable.

---

#### `sscanf(char *s, const char *format, ...)`
Parses data from the string `s`, similarly to how `scanf` reads from input.

---

#### `fgets(char *s, int n, FILE *stream)`
Reads up to `n-1` characters from `stream` into `s`, stopping at newline or EOF. 
Adds a null terminator. Newline is included if read. You can pass `stdin` as the 
stream argument to `fgets`, allowing it to read from standard input (i.e., 
the keyboard). This makes it a safer and more flexible alternative to `scanf`,
especially when reading full lines of input.

Unlike `scanf`, which stops at the first space and can leave unread characters in 
the input buffer, `fgets` reads the entire line up to the newline character, including 
spaces. 

When combined with `sscanf`, you can safely read and validate user input 
without risking buffer overflows or leftover input disrupting future reads.

```c
#include <stdio.h>

int main() {
    char buffer[100];
    int value;

    printf("Enter an integer: ");
    if (fgets(buffer, sizeof(buffer), stdin)) { // reads entire line from stdin (leaving std in clean)
        if (sscanf(buffer, "%d", &value) == 1) {
            printf("You entered: %d\n", value);
        } else {
            printf("Invalid input! Please enter a number.\n");
        }
    }
    return 0;
}
```

---

## String-Manipulation Functions (`<string.h>`)

The string-handling library provides useful functions for dealing with strings

### Manipulating string data 

Most functions automatically append the null terminator (`\0`) to the result.
Exception: `strncpy` only does so if space permits.

---

#### `strcpy(char *s1, const char *s2)`
Copies `s2` into `s1`, including the null terminator.

Returns a pointer to s1, which allows cascading (passing the result to another function)
e.g., `printf("%s", strcpy(...))`.

---

#### `strncpy(char *s1, const char *s2, size_t n)`
Copies at most `n` characters from `s2` into `s1`. Null-terminates only if `s2` fits.

After using you should probably always do `s1[n-1] = '\0'` to make sure that the 
NUL is there.

---

#### `strcat(char *s1, const char *s2)`
Appends `s2` to the end of `s1`, overwriting `s1`'s null terminator.

---

#### `strncat(char *s1, const char *s2, size_t n)`
Appends up to `n` characters of `s2` to `s1`.

---

### Comparing strings
These functions compare two strings lexicographically (i.e., based on character 
order in the ASCII table).

They return:

- `0` if the strings are equal
- a *negative* value if the first string is less than the second
- a *positive* value if the first string is greater than the second

--- 

#### `strcmp(const char *s1, const char *s2)`
Compares entire contents of `s1` and `s2` and then returns the result.

---

#### `strncmp(const char *s1, const char *s2, size_t n)`
Same as `strcmp`, but compares up to `n` characters.

---

### Searching within strings

---

#### `strchr(const char *s, int c)`
Finds the first occurrence of `c` in `s`.

---

#### `strrchr(const char *s, int c)`
Finds the last occurrence of `c` in `s`.

---

#### `strpbrk(const char *s1, const char *s2)`
Finds the first occurrence in `s1` of any character from `s2`.
<br>`pbrk` is short for **point**er to **break** (vaya con dios).

---

#### `strstr(const char *s1, const char *s2)`
Finds the first occurrence of the substring `s2` in `s1`.

---

#### `strspn(const char *s1, const char *s2)`
Returns the length of the initial segment of `s1` containing only characters from `s2`.
<br> `spn` is short for span.

---

#### `strcspn(const char *s1, const char *s2)`
Returns the length of the initial segment of `s1` that does *not* contain any 
characters from `s2`.
<br> `cspn` is short for *complement* span.

---

### Tokenizing strings (splitting them into pieces)

--- 

#### `strtok(char *s1, const char *s2)`
Splits `s1` into tokens using characters in `s2` as delimiters. Modifies `s1` 
by inserting `\0`. Uses static memory to remember the current position.
`*strtok` modifies the string by placing a `\0` after the first token.
Therefore we should make a copy of the string if we intend to use it after
calling `strtok`. 

It is *stateful*, meaning that it uses internal static storage to remember 
where it left off by saving a pointer to the byte *following* the one it just
replaced by `\0`. 

After the first call to `*strtok` we need to pass `NULL` to continue tokenizing 
the same string (it will then use the static memory to find where it left off).
If the byte it returns to is one of the separators, it will skip ahead to the 
next non-separator before continuing. 

If we pass a new string to `*strtok` it will reset the internal static storage 
which will effectively start a new sequence.

---

### Memory Functions

These functions manipulate, compare, or search raw memory, not just 
null-terminated strings. They work on `void *` pointers and use an explicit 
**byte** count (`size_t n`), which means they:

- Do not check for null terminators
- Work with ***any*** data, not just strings


Since `void *` can’t be dereferenced, you must always pass the number of bytes explicitly.

Note: Because these functions use `size_t n` to specify how many bytes to process,
you should typically use `sizeof(type)` multiplied by the number of elements to 
make sure you return the correct number of elements. 

---

#### `memcpy(void *s1, const void *s2, size_t n)`
Copies `n` bytes from `s2` to `s1`. Does not handle overlaps.
According to Bob "pros use this". 
Its main advantage is that, rather than copying byte-by-byte, it will copy data
in large chunks (such as 4, 8, or even 16–64 bytes at a time), often using SIMD
instructions or wide registers where supported by the architecture. If the 
number of remaining bytes is smaller than the chunk size, memcpy will fall 
back to copying those tail bytes one at a time (or in smaller groups like 2 or 
4 bytes), depending on alignment and platform optimizations.

---

#### `memmove(void *s1, const void *s2, size_t n)`
Safely copies `n` bytes from `s2` to `s1`, handling overlaps.
Unlike `memcpy`, memmove guarantees that the copy will work even if `s1` and `s2`
refer to parts of the same buffer.

Use `memmove` when the source and destination may overlap, such as when:

- You're shifting elements within an array
- One pointer is a subregion (subscript) of the other

---

#### `memcmp(const void *s1, const void *s2, size_t n)`
Compares `n` bytes from two memory regions. 
Returns `0` if equal, <br>`<0` if `s1` < `s2`, <br>`>0` if `s1` > `s2`.

---

#### `memchr(const void *s, int c, size_t n)`
Searches the first `n` bytes of `s` for `c`. Returns a pointer to the match or `NULL`.

---

#### `memset(void *s, int c, size_t n)`
Fills the first `n` bytes of `s` with byte value `c`.
Bob says "pros" also use this..

---

## Measuring String Length

---

#### `strlen(const char *s)`
Returns a `size_t` representing the length of string `s`, excluding the null terminator (`\0`).

---


