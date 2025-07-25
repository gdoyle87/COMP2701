# Week 10

This week just covers a bunch of character and string functions.


## Character-handling Library (`<ctype.h>`)

Each function in the `<ctype.h>` library accepts an `unsigned char` (which can
be represented as an `int`) or `EOF` (`EOF` is usually -1) as input. 

### Character Type Checking Functions (digits, letters, hex)

---

#### `isdigit(int c)`
Returns true if `c` is a digit (0–9), otherwise returns false.

---

#### `isalpha(int c)`
Returns true if `c` is a letter (a–z or A–Z), otherwise returns false.

---

#### `isalnum(int c)`
Returns true if `c` is either a digit or a letter, otherwise returns false.

---

#### `isxdigit(int c)`
Returns true if `c` is a valid hexadecimal digit (0–9, a–f, A–F).

---

### Case Checking and Conversion

---

#### `islower(int c)`
Returns true if `c` is a lowercase letter.

---

#### `isupper(int c)`
Returns true if `c` is an uppercase letter.

---

#### `tolower(int c)`
If `c` is an uppercase letter, returns its lowercase equivalent. Otherwise, returns `c` unchanged.

---

#### `toupper(int c)`
If `c` is a lowercase letter, returns its uppercase equivalent. Otherwise, returns `c` unchanged.

---

### Whitespace and Control Checking

#### `isblank(int c)`
Returns true if `c` is a space or tab.

---

#### `isspace(int c)`
Returns true if `c` is any whitespace character (space, tab, newline, vertical tab, form feed, carriage return).

---

#### `iscntrl(int c)`
Returns true if `c` is a control character (e.g., newline, tab, backspace).

---

#### `ispunct(int c)`
Returns true if `c` is a printable character but not a space or alphanumeric character.

---

### Printing Character Checking
A printing character is any character that results in a visible symbol on screen
or paper. This includes letters, digits, punctuation, and symbols.

---

#### `isprint(int c)`
Returns true if `c` is any printable character, including space.

---

#### `isgraph(int c)`
Returns true if `c` is any printable character except space.

---

## String Conversion Functions (`<stdlib.h>`)

The standard library provides functions that convert the ***initial part*** of a 
string to an integer or floating-point number. Parsing stops as soon as a character 
is encountered that is not valid for the specified number format.

If the input string does *not* start with a valid numeric sequence, the function 
returns `0` and `*endPtr` is set to point to the original `nPtr` (input).

If the base (for functions that have a base) is set to `0` then the function will
auto-detect the base.

If the number starts with `0` it will be interpreted as **octal**.
If the number starts with `0x` or `0X` it will be interpreted as **hex**.
If there is no prefix it will be interpreted as **decimal**.

The base can be any number up to 36 (10 digits + 26 characters).

---

#### `strtod(const char *nPtr, char **endPtr)`
Converts `nPtr` to a `double`. Stops at the first invalid character and sets `endPtr` to point to it.

---

#### `strtol(const char *nPtr, char **endPtr, int base)`
Converts `nPtr` to a `long`. Accepts a numerical base (e.g., 10 for decimal, 16 for hex).

---

#### `strtoul(const char *nPtr, char **endPtr, int base)`
Converts `nPtr` to an `unsigned long`. Otherwise behaves like `strtol`.Each function takes a second argument `endPtr`, which is used to return a pointer 
to the ***rest*** of the string (i.e., the character immediately after the parsed
number). If you don't need this information, you can pass `NULL` instead.

---


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
Formats a string like `printf`, but stores it in the array `s`.

---

#### `sscanf(char *s, const char *format, ...)`
Parses data from the string `s`, similarly to how `scanf` reads from input.

---

#### `fgets(char *s, int n, FILE *stream)`
Reads up to `n-1` characters from `stream` into `s`, stopping at newline or EOF. Adds a null terminator. Newline is included if read.
You can pass `stdin` as the stream argument to `fgets`, allowing it to read from
standard input (i.e., the keyboard). This makes it a safer and more flexible alternative 
to `scanf`, especially when reading full lines of input.

Unlike `scanf`, which stops at the first space and can leave unread characters in 
the input buffer, `fgets` reads the entire line up to the newline character, including 
spaces. When combined with `sscanf`, you can safely read and validate user input 
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
Returns 0 if `s1` equals `s2`, <0 if `s1` < `s2`, >0 if `s1` > `s2`.

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

---

#### `strstr(const char *s1, const char *s2)`
Finds the first occurrence of the substring `s2` in `s1`.

---

#### `strspn(const char *s1, const char *s2)`
Returns the length of the initial segment of `s1` containing only characters from `s2`.

---

#### `strcspn(const char *s1, const char *s2)`
Returns the length of the initial segment of `s1` that does *not* contain any characters from `s2`.

---

### Tokenizing strings (splitting them into pieces)

--- 

#### `strtok(char *s1, const char *s2)`
Splits `s1` into tokens using characters in `s2` as delimiters. Modifies `s1` by inserting `\0`. Uses static memory to remember the current position.`*strtok` modifies the string by placing a `\0` after the first token.
Therefore we should make a copy of the string if we intend to use it after
calling `strtok`. 

It is also *stateful*, meaning that it uses internal static storage to remember 
where it left off.

After the first call to `*strtok` we need to pass `NULL` to continue tokenizing 
the same string (it will then use the static memory to find where it left off)

If we pass a new string to `*strtok` it will reset the internal static storage 
which will effectively start a new sequence.

---

### Memory Functions

These functions manipulate, compare, or search raw memory, not just 
null-terminated strings. They work on `void *` pointers and use an explicit 
byte count (`size_t n`), which means they:

- Do not check for null terminators
- Work with ***any*** data, not just strings


Since `void *` can’t be dereferenced, you must always pass the number of bytes explicitly.

---

#### `memcpy(void *s1, const void *s2, size_t n)`
Copies `n` bytes from `s2` to `s1`. Does not handle overlaps.

---

#### `memmove(void *s1, const void *s2, size_t n)`
Safely copies `n` bytes from `s2` to `s1`, handling overlaps.

---

#### `memcmp(const void *s1, const void *s2, size_t n)`
Compares `n` bytes from two memory regions. Returns 0 if equal, <0 if `s1` < `s2`, >0 if `s1` > `s2`.

---

#### `memchr(const void *s, int c, size_t n)`
Searches the first `n` bytes of `s` for `c`. Returns a pointer to the match or `NULL`.

---

#### `memset(void *s, int c, size_t n)`
Fills the first `n` bytes of `s` with byte value `c`.

---

## Measuring String Length

---

#### `strlen(const char *s)`
Returns a `size_t` representing the length of string `s`, excluding the null terminator (`\0`).

---

