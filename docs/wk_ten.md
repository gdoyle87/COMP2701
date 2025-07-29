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

Each function takes a second argument `endPtr`, which is used to return a pointer 
to the ***rest*** of the string (i.e., the character immediately after the parsed
number). If you don't need this information, you can pass `NULL` instead.

---

#### `strtod(const char *nPtr, char **endPtr)`
Converts `nPtr` to a `double`. Stops at the first invalid character and sets `endPtr` to point to it.

---

#### `strtol(const char *nPtr, char **endPtr, int base)`
Converts `nPtr` to a `long`. Accepts a numerical base (e.g., 10 for decimal, 16 for hex).

---

#### `strtoul(const char *nPtr, char **endPtr, int base)`
Converts `nPtr` to an `unsigned long`. Otherwise behaves like `strtol`.

---


