# Midterm Review 

## Six Phases of C Program Development
**Editing/Programming**: Write source code.
<br>**Preprocess**: Handle directives like `#include` (copies file contents) and `#define` (text replacement).
<br>**Compile**: Check syntax, generate `.o` object files with machine code.
<br>**Link**: Combine `.o` files and C library into an executable.
<br>**Load**: OS loads executable into memory.
<br>**Execute**: CPU runs program starting from `main()`.

## Main Function
Every C program requires a `main()` function.
```c
#include <stdio.h>
int main(void) {
    printf("Hello World\n");
    return 0;
}
```

## Variables
**Statically Typed**: Declare type before use (e.g., `int`, `float`, `char`).

When naming variables, they must start with letter/underscore and can contain letters/digits/underscores.


```c
int a = 3;
float b = 5.5;
char c = 'A';
```

## Preprocessor Directives
**#include**: Import library functions (e.g., `#include <stdio.h>`).
<br>**#define**: Define symbolic constants to avoid magic numbers.
```c
#define MAX 100
for(int i = 0; i < MAX; ++i) { ... }
```

## Input/Output

**Output**:

- `puts()`: Prints string with automatic newline.

- `printf()`: Uses format specifiers (`%d`, `%c`, `%f`) for dynamic output, no automatic newline.
```c
printf("You entered %d.\n", number);
```

**Input**:

 - `scanf()`: Reads input with format specifiers, requires variable address (`&`) except for strings.
```c
int num;
scanf("%d", &num);
char name[20];
scanf("%19s", name); // No & for strings
```

## Operators
**Arithmetic**: `+`, `-`, `*`, `/`, `%` 
<br>**Assignment**: `=`, `+=`, `-=`, `*=`, `/=`, `%=`.
<br>**Comparison**: `==`, `!=`, `<`, `>`, `<=`, `>=` (return 1 or 0).
<br>**Logical**: `&&` (AND), `||` (OR), `!` (NOT), uses short-circuit evaluation.

**Note**: We can use casting to convert types for precision (for example, keeping decimal
places when dividing two integers by casting one to a float).

    float result = (float)2 / 3; // 0.67
    printf("%.2f\n", result);

## Algorithms
Procedure for solving a problem with *actions to execute* and *execution order* of those actions (program control).

## Control Structures
Three types of control structures: **sequence**, **selection**, and **iteration**.

### Sequence
Statements execute in order.

### Selection
Choose based on condition.
 `if`, `if...else`, `switch`: 

  - **Ternary Operator** (`?:`) is a concise `if...else`.
```c
    puts(x > 0 ? "good" : "bad");
```

### Iteration 
Repeat until condition met.

`for`, `while`, `do...while` 


## Increment/Decrement
**Pre-increment** (`++x`): Increment first.
<br>**Post-increment** (`x++`): Use, then increment.

Bob insists we use pre-increment in loop continuation conditions:

```c
for(int i = 0; i < 10; ++i) { ... }
```

## Functions
Self-contained code for specific tasks.

They help:

- **Divide and Conquer**: Build programs from reusable parts.
- **Abstraction**: Hide implementation details behind function names.
- **Decomposition**: Break complex problems into smaller functions.

A function is like a worker. It is invoked/called by another function which can be thought of as its boss.

## Function Definition
```c
return_type function_name(parameters) { statements }
```

Example:
```c
int square(int number) { return number * number; }
```

## Function Prototype
Due to C being sequential, we must declare function before use to avoid errors.
A prototype allows us to declare a function up front and define it later.
<br>Example:
```c
int square(int);
...
int square(int i)
{
  return i*i;
}
```

Note: the variable name used in the prototype is meaningless (and can be skipped)
only the type matters.

<br>**Argument Coercion**: When passing arguments to a function or assigning 
between variables, C will automatically convert values to the expected type if needed.
This may cause data loss (e.g., `double` to `int` drops decimals).

## Function-Call Stack
The function call stack is a last-in, first-out (LIFO) structure for function calls/returns.

In the function call stack, we ***push*** a frame with return address on function call and ***pop*** from the stack on return.

If you do excessive pushes to the stack, we will get a Stack overflow when we run out of space in memory.

## Pass Arguments
**By Value**: Copies argument; original unchanged. All arguments in C are passed by value.

```c
void superFunction(int num) { num *= 1000; } // No effect on original
```

**By Reference**: Modifies original. Need to use pointers to achieve this in C.

## Random-Number Generation
From `<stdlib.h>`:

 - `rand()`: Returns an int between 0 and `RAND_MAX` (≥32,767).
 - `srand(seed)`: Sets seed.

Use mod `%` and the number of values your range is in. Offset it as needed.
```c
int dice = 1 + (rand() % 6); // 1 to 6
```

Use `srand(time(NULL))` for unique sequences.

## Enumerated Types
Programmer-defined types listing all valid values.

```c
enum DaysOfWeek {SUN, MON, TUES, WED, THU, FRI, SAT};
enum DaysOfWeek today = MON; // Assigns 1
```

Default starts at 0, increments by 1; can set explicit values.
```c
enum Status {CONTINUE = 0, WON = 1, LOST = 2};
```

## Storage Classes
Storage classes set some attributes of variables such as: **storage duration** (lifetime), **scope** (visibility), **linkage** (file access).

`auto`: Default for local variables; stored on stack (rarely used explicitly).
<br>`register`: Requests CPU register storage (mostly obsolete; ignored by modern compilers).
<br>`static`: Persistent lifetime; local retains value between calls, global restricts to file.
<br>`extern`: Declares variable/function in another file; no storage created.

## Scope
**Function Scope**: Labels (e.g., `goto`, `switch`) visible in function.
<br>**File Scope**: Global variables, function prototypes outside functions; visible from declaration to file end.
<br>**Block Scope**: Variables in `{}` 
<br>**Function-Prototype Scope**: Parameter names in prototypes (optional, ignored by compiler).

## Memory Regions
**Stack**: Automatic variables; allocated/deallocated with block scope.
<br>**Static**: Global/static local variables; persist entire program, initialized once.
<br>**Heap**: Dynamic memory (via `malloc()`, `calloc()`, `realloc()`); manually managed.

## Arrays
Group of same-type elements stored contiguously in memory.

Use array name with 0-based index to access an element (e.g., `my_array[3]` for fourth element).

```c
int my_arr[3]; // Static size, cannot be resized
```

**Best Practice**: Use `#define` for array size to avoid magic numbers.

**Looping**: Use `size_t` for index in loops.
```c
for (size_t i = 0; i < 5; ++i) { n[i] = i; }
```

**Initializer Lists**:
```c
int n[5] = {32, 27, 64, 18, 95}; // Sets values
int n[5] = {0}; // Sets all to 0
```

## Strings
Array of `char` terminated by `'\0'` (NUL).

```c
char s1[] = "hello"; // Auto-sized, NUL added
char s2[10] = "hello"; // Fixed size, must fit NUL
char s3[] = {'h', 'e', 'l', 'l', 'o', '\0'}; // Manual NUL

char buf[20];
scanf("%19s", buf); // No & needed, size limits input
```

## Static vs Automatic Arrays
**Static Array**: Single instance, persists entire program, initialized to 0.
<br>**Automatic Array**: New instance per function call, contains garbage unless initialized.
```c
static int s_arr[3]; // Persists, 0-initialized
int a_arr[3]; // New each call, garbage values
```

## Passing an Array to a Function
Use array name without brackets (e.g., `myFunc(myArr)`).

Passes pointer to first element; function modifies original array (pass by reference).

**Const Arrays**: Use `const` to prevent modification (e.g., for searching).

```c
void searchArray(const int arr[], size_t size);
```

## Multi-Dimensional Arrays
Arrays of arrays (e.g., 2D array like a spreadsheet). First index is row, second is column (e.g., `arr[row][col]`).

Stored in row-major order (row by row) in contiguous 1D memory.


**Initializer Lists**:
```c
int arr[3][3] = {{0, 1, 2}, {3, 4, 5}, {6, 7, 8}}; // Readable
int arr[3][3] = {0, 1, 2, 3, 4, 5, 6, 7, 8}; // Same memory layout
```

**Passing to Functions**: You must specify all dimension sizes except first (e.g., `void printArray(int arr[][3], size_t nRows)`).

**Looping**: Use descriptive indices (e.g., `row`, `col`) instead of `i`, `j`.
```c
for (size_t row = 0; row < 3; ++row) {
    for (size_t col = 0; col < 3; ++col) {
        printf("%d ", arr[row][col]);
    }
    printf("\n");
}
```

## Variable-Length Arrays (VLAs)
Arrays sized at runtime (e.g., `int arr[size]` where `size` is user input).

Bob says **do not use** in this course.

## Functions to be able to create from Weeks 5 and 6
<details>
<summary>Find max value in array</summary>

```c
int arr_max(const int a[], size_t n)
{
    int max = a[0];
    for (unsigned int i = 1; i < n; ++i)
    {
        if (a[i] > max)
        {
            max = a[i];
        }
    }
    return max;
}
```

</details>

<details>
<summary>Index of first max value</summary>

```c
size_t arr_index_of_first_max(const int a[], size_t n)
{
    int max = a[0];
    int index = 0;
    for (unsigned int i = 1; i < n; ++i)
    {
        if (a[i] > max)
        {
            max = a[i];
            index = i;
        }
    }
    return index;
}
```

</details>

<details>
<summary>Index of last max value</summary>

```c
size_t arr_index_of_last_max(const int a[], size_t n)
{
    int max = a[0];
    int index = 0;
    for (unsigned int i = 1; i < n; ++i)
    {
        if (a[i] >= max)
        {
            max = a[i];
            index = i;
        }
    }
    return index;
}
```

</details>

<details>
<summary>Index of first occurrence of value</summary>

```c
int arr_index_of_value_first(const int a[], size_t n, int value)
{
    for (unsigned int i = 0; i < n; ++i)
    {
        if (a[i] == value)
        {
            return i;
        }
    }
    return -1;
}
```

</details>

<details>
<summary>Index of last occurrence of value</summary>

```c
int arr_index_of_value_last(const int a[], size_t n, int value)
{
    int index = -1;
    for (unsigned int i = 0; i < n; ++i)
    {
        if (a[i] == value)
        {
            index = i;
        }
    }
    return index;
}
```

</details>

<details>
<summary>Compare two arrays</summary>

```c
int arr_compare(const int a[], const int b[], size_t n)
{
    for (unsigned int i = 0; i < n; ++i)
    {
        if (a[i] != b[i])
        {
            return 0;
        }
    }
    return 1;
}
```

</details>

<details>
<summary>Find first occurrence of char</summary>

```c
int find_first(const char s[], char c)
{
    for (int i = 0; s[i] != '\0'; ++i)
    {
        if (s[i] == c)
        {
            return i;
        }
    }
    return -1;
}
```

</details>

<details>
<summary>Replace last occurrence of char</summary>

```c
int replace_last(char s[], char oldChar, char newChar)
{
    int index = -1;
    for (int i = 0; s[i] != '\0'; ++i)
    {
        if (s[i] == oldChar)
        {
            index = i;
        }
    }
    if (index != -1)
    {
        s[index] = newChar;
    }
    return index;
}
```

</details>

<details>
<summary>Count number of occurrences of char</summary>

```c
int count_occ(const char s[], char c)
{
    int count = 0;
    for (int i = 0; s[i] != '\0'; ++i)
    {
        if (s[i] == c)
        {
            ++count;
        }
    }
    return count;
}
```

</details>

<details>
<summary>Count alphabetic characters</summary>

```c
int count_alpha(const char s[])
{
    int count = 0;
    for (int i = 0; s[i] != '\0'; ++i)
    {
        if (isalpha(s[i]))
        {
            ++count;
        }
    }
    return count;
}
```

</details>

<details>
<summary>Check if string is all digits</summary>

```c
int is_all_digits(const char s[])
{
    for (int i = 0; s[i] != '\0'; ++i)
    {
        if (!isdigit(s[i]))
        {
            return 0;
        }
    }
    return 1;
}
```

</details>

<details>
<summary>Copy string to lowercase</summary>

```c
void lowercase_copy(char dest[], const char src[])
{
    int i;
    for (i = 0; src[i] != '\0'; ++i)
    {
        dest[i] = tolower(src[i]);
    }
    dest[i] = '\0';
}
```

</details>

<details>
<summary>Validate BCIT ID</summary>

```c
int is_valid_bcit_id(const char id[])
{
    if (id[0] != 'a' && id[0] != 'A')
    {
        return 0;
    }
    int i;
    for (i = 1; (id[i] != '\0') && (i < 10); ++i)
    {
        if (!isdigit(id[i]))
        {
            return 0;
        }
    }
    if (i != 9)
    {
        return 0;
    }
    return 1;
}
```

</details>

<details>
<summary>Return first word</summary>

```c
void return_first_word(const char input[], char output[])
{
    while ((*input != '\0') && (isspace(*input)))
    {
        ++input;
    }
    while ((*input != '\0') && (!isspace(*input)))
    {
        *output++ = *input++;
    }
    *output = '\0';
}
```

</details>

<details>
<summary>Return second word</summary>

```c
void return_second_word(const char input[], char output[])
{
    while ((*input != '\0') && (isspace(*input)))
    {
        ++input;
    }
    while ((*input != '\0') && (!isspace(*input)))
    {
        ++input;
    }
    while ((*input != '\0') && (isspace(*input)))
    {
        ++input;
    }
    while ((*input != '\0') && (!isspace(*input)))
    {
        *output++ = *input++;
    }
    *output = '\0';
}
```

</details>

<details>
<summary>Count number of words</summary>

```c
int return_count_of_words(const char input[])
{
    int count = 0;
    while ((*input != '\0') && (isspace(*input)))
    {
        ++input;
    }
    while (*input != '\0')
    {
        ++count;
        while ((*input != '\0') && (!isspace(*input)))
        {
            ++input;
        }
        while ((*input != '\0') && (isspace(*input)))
        {
            ++input;
        }
    }
    return count;
}
```

</details>

<details>
<summary>Return nth word</summary>

```c
void return_nth_word(const char input[], char output[], int n)
{
    while ((*input != '\0') && (isspace(*input)))
    {
        ++input;
    }
    int count = 1;
    while ((*input != '\0') && (count < n))
    {
        while ((*input != '\0') && (!isspace(*input)))
        {
            ++input;
        }
        while ((*input != '\0') && (isspace(*input)))
        {
            ++input;
        }
        ++count;
    }
    while ((*input != '\0') && (!isspace(*input)))
    {
        *output++ = *input++;
    }
    *output = '\0';
}
```

</details>

