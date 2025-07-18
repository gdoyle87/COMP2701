# Week 7

## Dynamic Memory Allocation
Up until now we have been ***statically*** creating memory, meaning that the memory size is 
fixed at compile time (e.g. arrays with a fixed length). In practice, this can 
cause some issues. For instance, if you declare `int arr[100]` for user input 
but only need 10 elements, you waste memory. If the user needs 200 elements,
your program crashes.

***Dynamic memory allocation*** lets you request memory at runtime, which allows data
structures that can grow/shrink (linked lists, dynamic arrays, trees).

In `Java`, using the `new` keyword, created a dynamic object in memory with automatic
 **garbage collection**. This means that when an object is no longer needed, its
memory is automatically cleaned up, like throwing away garbage, so the space can
be reused.

In `C`, there is no automatic garbage collection. We must manage memory 
manually, cleaning up our own “garbage” by freeing memory when we’re done with
it, or else we risk memory leaks.

### Allocating Memory
In C we have 3 functions for allocating memory: `malloc()`, `calloc()`, and `realloc()`.

#### The **`malloc(size)` function**
Allocates a block of *uninitialized* memory of the specified
size in bytes. 

If it is *successful* we will be returned a pointer of type `void*` (which can be
assigned to *any* pointer type). Therefore we should assign it to a pointer variable:
```c
int *dynamicArray = NULL; //create an int pointer
dynamicArray = malloc(sizeof(int) * 4); // use malloc to allocate and assign the array
if (dynamicArray == NULL) // check that malloc did not fail
{
    return EXIT_FAILURE;
}
```

If it is *not* successful it will return `NULL`.
<br>***Always*** check if the return from `malloc()` is `NULL` to avoid errors.

#### The `calloc()` and `realloc()` Functions
**`calloc(n, size)`** allocates space for an array of *n* elements of the 
specified size **and** initializes all bits to zero. This can be helpful for 
arrays where we want to be sure values are initialized. *However* this comes at 
the cost of some overhead.

**`realloc(ptr, new_size)`** changes the size of a previously allocated block.


#### Deallocating Memory with `free()`
When we no longer need a block of dynamically allocated memory, we can use the 
`free(myPtr)` function to deallocate the memory and return it to the system. 

If we don't use `free()` we could run out of memory causing a **memory leak** which
will likely crash our program. 

After we free a pointer, we should set the pointer to `NULL` so that if we 
accidently refer to that pointer later, we don't refer to invalid memory.
```c
free(ptr);
ptr = NULL;
```

This also helps us to avoid freeing memory twice. If we free twice we might end up 
freeing memory that is being used by something else by mistake.

`free()` should ***only*** be used with memory we allocated with `malloc()`, `calloc()`,
or `realloc()`. Using it on static or stack variables is undefined behaviour.

#### Allocating the Right Amount of Space with `sizeof`
We use `sizeof` to make sure we allocate the correct amount of memory.

```c
int var1;
size_t len = sizeof var1; // will give us 4 bytes of memory (the size of an int)
```

Note: If you use sizeof with a pointer you will get the size of a pointer in
your system (e.g. 8 bytes on a 64-bit machine) and *not* the size of the underlying 
value it points to.

So when allocating space for an array dynamically, always use sizeof on the type, not the pointer:
```c
int *arr = malloc(sizeof(int) * len);
```

## Pointers and the Heap
A pointer is a variable that stores the memory address of another variable or block of memory.

When you declare a pointer, you use the `*` operator to indicate that the variable is a pointer type:
```c
int *iPtr = NULL; // should always initialize a pointer even if its just to NULL
```
The pointer itself lives on the stack, but it can point to a value in the heap (or anywhere in memory).


Once we call `malloc()` such as `iPtr = malloc(sizeof(int));` we are creating a 
reference to a place in the heap that will be used for this value.

When we are dealing with a single scalar (a simple data value - not an array or
struct), in order to access the variable on the heap we need to *dereference*
the pointer. The dereference operator is `*`. 

The `*` symbol has two jobs in C:

1. When you declare a pointer, `*` marks the variable as a pointer type.
2. When you use it with a pointer variable, `*` means "follow this pointer to get the value."

## Dynamic Arrays and Pointers
When you allocate an array with malloc or calloc, the pointer points to the first element:
```c
int *arr = malloc(sizeof(int) * len);
```

You can use this pointer like an array:
```c
arr[0] = 1;
arr[1] = 2;
```

Remember: `sizeof(arr)` only gives the size of the pointer, not the total size 
of the array in memory.

## Dynamic Memory for Strings
One common use of dynamic memory is handling strings of unknown length.

For example, imagine we’re reading an input that we know will never exceed 80 
characters. We’d declare a `char` array of length **81** (80 characters plus 
the terminating `NUL`). But if the user only types `"hello"` (6 bytes including
`'\0'`), you’ve still reserved 81 bytes,75 of which go unused.

We can use dynamic memory allocation to avoid most of that waste. The typical
approach has two steps: create a fixed buffer on the stack, then copy it into a 
`malloc`’d block on the heap.

<ol>
  <li>
    Read into the stack buffer

    <pre><code class="language-c">
char buffer[81];
scanf("%80[^\n]", buffer); // read up to 80 chars or until newline
getchar();                 // consume the leftover '\n'
    </code></pre>
  </li>

  <!-- force the second item to be “2.” even if your renderer auto-renumbers -->
  <li value="2">
    Copy into a heap block sized exactly to the string

    <pre><code class="language-c">
size_t len = strlen(buffer);
char *userString = malloc(len + 1);
strcpy(userString, buffer);
// …do a bunch of stuff and use userString…
free(userString);
    </code></pre>
  </li>
</ol>



In this example you still “waste” 81 bytes on the stack, but only once. Every 
time you read another line, you reuse that same buffer, and each `malloc`'d 
string on the heap is exactly the length you need (plus one).


#### Note: scanset
In the example above, `%80[^\n]` uses a `scanset` to control how `scanf` reads input.

A `scanset`, written inside square brackets `[ ]`, tells `scanf` to match a 
custom set of characters.

The `^` means **"not"**,  so `[^\n]` means “match any characters that are not a newline.”

You can think of a scanset as a custom format specifier. 
It works like a very simple character class, similar to part of a regular expression,
but limited to defining which characters to include or exclude.


## Leetcode 66

Remember that when we pass an array to a function, inside the function it
behaves as a pointer to the first element.

In this question, `int* plusOne(int* digits, ...)` — `digits` is an array
argument that decays to a pointer.

The `digits` array represents a single number, with each element holding one 
digit of that number. We are adding 1 to this number.

The challenge is that adding 1 might cause a carry that affects multiple digits.
In some cases, like `[9, 9, 9]`, the result needs to be one digit longer 
(e.g., 999 + 1 = 1000 or `[1,0,0,0]`).

Bob gave us this starter code, which works for simple cases that do not
require carrying past the first digit. Our goal is to handle the cases 
where the result needs an extra digit:

```c
int* plusOne(int* digits, int digitsSize, int* returnSize) {
    // allocate space for the result array
    int *resultPtr = malloc(sizeof(int) * digitsSize);

    // populate our result array with the values of digits
    for (size_t i = 0; i < digitsSize; ++i) {
        resultPtr[i] = digits[i];
    }

    // add one to the *final* element in the array
    ++resultPtr[digitsSize - 1];

    // set the value of returnSize to digitsSize by *dereferencing* returnSize.
    *returnSize = digitsSize;

    // If the increment causes a carry past the first digit,
    // we will need to make *returnSize = digitsSize + 1

    return resultPtr;
}
```
