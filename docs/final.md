# Final Exam Review

[Review the Midterm Notes](midterm.md/)

## Dynamic Memory Allocation

C lets you request memory at **runtime** using functions like `malloc()` instead of hardcoding array sizes at compile time. This avoids wasted memory and allows data structures like lists and dynamic arrays.

C has **no garbage collection** — you must manually free memory when done using `free()`.

### `malloc(size)`
Allocates a block of **uninitialized** memory.

```c
int *arr = malloc(sizeof(int) * 4);
if (arr == NULL) { return EXIT_FAILURE; }
```

### `calloc(n, size)`
Like `malloc`, but zero-initializes the memory.

```c
int *arr = calloc(4, sizeof(int));
```

### `realloc(ptr, new_size)`
Changes the size of previously allocated memory.

### `free(ptr)`
Releases memory. Set pointer to `NULL` afterward to avoid dangling references.

```c
free(arr);
arr = NULL;
```

### `sizeof` with Pointers
```c
sizeof(ptr)     // size of the pointer (e.g. 8 bytes)
sizeof(*ptr)    // size of the pointed-to type (e.g. 4 bytes for int)
```

---

## Pointers and the Heap

A pointer stores the memory address of another variable. When dynamically allocating:

```c
int *ptr = malloc(sizeof(int));
*ptr = 10; // Dereference to assign
```

- Pointer itself lives on the **stack**
- Value it points to lives on the **heap**

---

## Dynamic Arrays

```c
int *arr = malloc(sizeof(int) * len);
arr[0] = 5;
```

Pointers to heap arrays behave like regular arrays, but `sizeof(arr)` returns pointer size.

---

## Dynamic Memory for Strings

To handle string input of unknown length:

```c
char buffer[81];
scanf("%80[^
]", buffer);
getchar(); // consume '
'

size_t len = strlen(buffer);
char *copy = malloc(len + 1);
strcpy(copy, buffer);
```

---

## Scanset
`%80[^ ]` tells `scanf` to read up to 80 characters until newline.  
- `[^...]`: match anything *except* characters inside

---

## Pointers

Pointers are variables that store memory addresses.

### Why Use Pointers?
C uses **pass-by-value**, so variables are copied when passed to functions.  
To modify the original variable, pass its **address** using pointers.

Pointers let us:
- Avoid global variables
- Share memory between functions
- Modify original values in functions

---

## Indirection

Using a pointer to access a value is called **indirection**.

```c
int count = 7;
int *countPtr = &count;
printf("%d", *countPtr); // prints 7
```

---

## Pointer Size

Pointers match the CPU register width:
- 32-bit systems: 4-byte pointers
- 64-bit systems: 8-byte pointers

---

## Declaring a Pointer

```c
int *ptr;
```

All of these are equivalent:
```c
int *ptr;
int* ptr;
int * ptr;
```

Avoid multiple declarations like:
```c
int *ptr, x; // only ptr is a pointer
```

Instead:
```c
int *ptr;
int x;
```

---

## Initializing a Pointer

Always initialize, even to `NULL`:

```c
int *ptr = NULL;
```

---

## The Address (`&`) Operator

Returns the memory address of a variable:

```c
int y = 5;
int *yPtr = &y;
```

---

## The Indirection (`*`) Operator

Dereferences the pointer to get the value:

```c
int x = 10;
int *ptr = &x;
printf("%d", *ptr); // prints 10
```

`*` means:
- In declarations: declare a pointer
- In expressions: dereference the pointer

---

## `const` with Pointers

`const` affects either the **data** or the **pointer**:

```c
const double *dPtr;      // can't change value pointed to
double *const dPtr;      // can't change the pointer itself
const double *const dPtr; // can't change either
```

---

## Incrementing a Pointer

```c
int arr[] = {1, 2, 3};
int *ptr = arr;
++ptr; // now points to arr[1]
```

Increment moves to the next element, not just next byte.

Pointer increment inside a function only affects the local copy unless passed as a pointer-to-pointer.

---

## `sizeof` Operator

`sizeof` is a **compile-time operator** (not a function) that returns the size (in bytes) of a type or object as a `size_t`.

```c
int array[20];
printf("%zu", sizeof array); // prints 80 if int is 4 bytes
```

- `sizeof(type)` requires parentheses.
- `sizeof(variable)` does not.

---

### Pointers and `sizeof`

When used on a pointer, `sizeof` returns the size of the pointer itself — **not** what it points to.

```c
int *p;
sizeof(p);     // size of pointer (e.g., 8 bytes)
sizeof(*p);    // size of int (e.g., 4 bytes)
```

Arrays decay to pointers when passed to functions, so `sizeof` no longer gives array size inside a function.

---

## Pointer Arithmetic

Valid operations:
- `++`, `--`
- `+` and `-` with integers
- subtracting one pointer from another

When you add `n` to a pointer, it advances by `n * sizeof(type)` bytes.

```c
myPtr += 2; // skips 2 elements, not 2 bytes
```

---

### Subtracting Pointers

```c
int *a, *b;
// suppose a = 0x1000, b = 0x1008, and sizeof(int) = 4
b - a; // result is 2 (elements apart)
```

---

## Pointing to Arrays

```c
int arr[] = {0, 1, 2};
int *ptr = arr;      // same as &arr[0]
```

---

## Offset Notation

```c
arr[3]      == *(arr + 3)
ptr[2]      == *(ptr + 2)
```

Always use parentheses: `*(arr + i)`  
Not: `*arr + i` (which adds after dereferencing)

---

## `void *` Pointers

Generic pointer type. Can point to any data type.

```c
void *vp;
int *ip = vp; // allowed with explicit cast
```

Cannot dereference a `void *` directly — must cast to a specific type first.

---

## Function Pointers

Functions have addresses just like data.

```c
void bubbleSort(int a[], size_t n, int (*cmp)(int, int));
```

Calling:

```c
(*cmp)(a, b);
```

Parentheses around `*cmp` are required to make it a pointer to a function.

---

## `<ctype.h>` – Character Classification

Each function takes an `int` (usually a `char` cast to `int`).

### Type Checking
- `isdigit(c)` – true if 0–9  
- `isalpha(c)` – true if a–z or A–Z  
- `isalnum(c)` – true if letter or digit  
- `isxdigit(c)` – true if valid hex (0–9, a–f, A–F)

### Case Checking and Conversion
- `islower(c)` – true if lowercase  
- `isupper(c)` – true if uppercase  
- `tolower(c)` – converts to lowercase  
- `toupper(c)` – converts to uppercase

### Whitespace & Control
- `isblank(c)` – space or tab  
- `isspace(c)` – any whitespace (space, tab, newline, etc.)  
- `iscntrl(c)` – control characters (e.g., newline, tab)  
- `ispunct(c)` – punctuation (not space or alphanumeric)

### Printable Characters
- `isprint(c)` – any printable char, including space  
- `isgraph(c)` – printable except space

---

## `<stdlib.h>` – String Conversion

### Conversion Functions

- `strtod(s, endPtr)` – string to `double`  
- `strtol(s, endPtr, base)` – string to `long`  
- `strtoul(s, endPtr, base)` – string to `unsigned long`

Behavior:
- Stops at first invalid char  
- `base = 0` auto-detects:  
  - `0x` or `0X` → hex  
  - `0` → octal  
  - otherwise → decimal  

---

## `<stdio.h>` – Input/Output

- `getchar()` – reads next char from stdin  
- `putchar(c)` – writes char to stdout  
- `puts(s)` – prints string with newline  
- `sprintf(dest, format, ...)` – formats into a string  
- `sscanf(s, format, ...)` – parses from string  
- `fgets(s, n, stream)` – reads up to n-1 chars, stops at newline or EOF, includes newline

Example (safe input):
```c
char buffer[100];
int value;

if (fgets(buffer, sizeof buffer, stdin)) {
    if (sscanf(buffer, "%d", &value) == 1) {
        printf("Value: %d\n", value);
    }
}
```

---

## `<string.h>` – String Handling

### Copying and Concatenation
- `strcpy(dest, src)` – copies string including NUL  
- `strncpy(dest, src, n)` – at most n chars; may not NUL-terminate  
- `strcat(dest, src)` – appends src to dest  
- `strncat(dest, src, n)` – appends at most n chars

### Comparing Strings
- `strcmp(s1, s2)` – full comparison  
- `strncmp(s1, s2, n)` – compares first n chars

### Searching
- `strchr(s, c)` – first occurrence of `c`  
- `strrchr(s, c)` – last occurrence of `c`  
- `strpbrk(s1, s2)` – first match of any char from s2  
- `strstr(s1, s2)` – first substring match  
- `strspn(s1, s2)` – length of segment with only s2 chars  
- `strcspn(s1, s2)` – length of segment with no s2 chars


### `strtok` – Tokenizing

```c
char *token = strtok(str, " ,.-");
while (token != NULL) {
    // process token
    token = strtok(NULL, " ,.-");
}
```

- Modifies input string
- Remembers position using internal static storage
- Start new string by calling again with non-NULL

---

### Memory Functions (Raw Byte Handling)

#### Work on any data, not just strings
- `memcpy(dest, src, n)` – fast copy, no overlap safety  
- `memmove(dest, src, n)` – overlap-safe copy  
- `memcmp(s1, s2, n)` – compares n bytes  
- `memchr(s, c, n)` – searches first n bytes for `c`  
- `memset(s, c, n)` – fills memory with byte `c`

---

### `strlen`

Returns length of a string excluding the null terminator.

```c
size_t len = strlen("hello"); // 5
```

---

