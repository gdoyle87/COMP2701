# Week 4

## Enumerated Types

Enumerated types are types created by the programmer themselves. They enumerate all the legal values that can be assigned to a variable of that type.

```c
enum DaysOfWeek {SUN, MON, TUES, WED, THU, FRI, SAT};
enum DaysOfWeek today = MON; // will assign 1 to today
```

<details open><summary>Example:</summary>
<img src="../enum.gif" alt="example enum animation">

<details><summary>Click to show code</summary>
```C
#include <stdio.h>

enum DaysOfWeek { SUN, MON, TUES, WED, THU, FRI, SAT };

const char *day_name(enum DaysOfWeek day) {
  switch (day) {
  case SUN:
    return "Sunday";
  case MON:
    return "Monday";
  case TUES:
    return "Tuesday";
  case WED:
    return "Wednesday";
  case THU:
    return "Thursday";
  case FRI:
    return "Friday";
  case SAT:
    return "Saturday";
  default:
    return "Invalid";
  }
}

int main(void) {
  printf("All days of the week:\n");
  for (enum DaysOfWeek d = SUN; d <= SAT; ++d) {
    printf("%d = %s\n", d, day_name(d));
  }

  return 0;
}
```
</details>
</details>

<br>
By default, the first name is assigned the integer value 0, and each subsequent name increases by 1.
<br>We can also explicitly set values.
```c
enum Status {CONTINUE = 0, WON = 1, LOST = 2};
```

## Storage Classes
Variables have other attributes beside the `name`, `type`, `size` and `value`. One such is the *storage class specifier* which we will go into below. 
<br>The storage class specifier is used to determine the following other attributes: ***storage duration*** (when it exists in memory), ***scope*** (where a program can reference the identifier), and ***linkage*** (whether it is known only in the current file or in any source) for the identifier.

`C` has the following **storage class specifiers**:

1. `auto`
    - This is the **default** for local (block-scope) variables. It tells the compiler to store the variable in automatic storage (i.e., on the stack).
    - It's largely redundant in modern C and rarely written explicitly.
2. `register`
    - This was used to request that a variable be stored in a CPU register for faster access.
    - You cannot get the address of a `register` variable using `&`.
    - Modern compilers ignore it and optimize register usage themselves, so this is now mostly obsolete.
3. `static`
    - Gives a variable static storage duration – it exists for the lifetime of the program, even if defined inside a function.
    - Two main uses:
        - **Static local variables**: retain their value between function calls.
        - **Static global variables/functions**: restrict visibility to the current translation unit (file) – no external linkage.
4. `extern`
    - Tells the compiler that a global variable or function exists in another file.
    - It does **not** create storage; it just *declares* the identifier so it can be used.
    - Common use: declare a global variable defined in another `.c` file.

## Scope
The scope of an identifier is the portion of the program in which that identifier can be referenced. In `C` there are four identifier scopes: 

1. *function* scope
	- Applies only to labels (e.g., `start:`) used with `goto` and `switch` statements.
	- Labels are visible anywhere in the function where they're defined, but *note* outside it.
	- They support information hiding, aligning with the *principle of least privilege*: only grant access needed for a specific task.
2. *file* scope
	- Applies to identifiers declared outside any function.
	- Visibility spans from the point of declaration to the end of the file.
	- Examples:
		- Global variables
		- Function prototypes
		- Function definitions outside functions
3. *block* scope
	-  Applies to identifiers declared inside a block (between `{` and `}`).
	- Blocks can *nest*, and inner blocks can hide outer block identifiers with the same name — generally best to avoid name hiding.
	- `static` local variables still have block scope (but static storage duration).
4. *function-prototype* scope
	- Applies only to parameter names in function prototypes - within the `()` of a prototype
	- These names are **optional** and **ignored** by the compiler — only types matter.

## Memory Regions Overview
Before we move on, it's useful to understand where variables actually live in memory. The location affects how long they last, how they're accessed, and how they're cleaned up.

| Stack               | Static                 | Heap               |
| ------------------- | ---------------------- | ------------------ |
| Automatic Variables | Global Variables       | Dynamic Variables* |
|                     | Static Local Variables |                    |

<small>*dynamic variables are created with `malloc()`, `calloc()`, and `realloc()`. </small>


**Stack:** Fast allocation/deallocation; tied to block scope. Stack variables are automatically deallocated when their scope ends.
<br>**Static:** Persistent for the program’s lifetime; only initialized once.
<br>**Heap:** Used for dynamic memory; you manage it explicitly.


### The Heap vs. A Heap
The term **heap** can refer to two separate concepts in programming — one related to memory, the other to data structures and algorithms.
<hr>
#### ***The*** heap (memory region):

A section of memory used for dynamic allocation. Variables stored here persist until manually deallocated (e.g., with `free()` in C).

- No ordering — not FIFO, not LIFO. It's just a pool of memory.

<hr> 
#### ***A*** heap (data structure):

A type of tree-based structure (e.g., min-heap or max-heap) used for priority queues.

- Has ordering rules (e.g., smallest or largest item always on top), but unrelated to memory allocation.
