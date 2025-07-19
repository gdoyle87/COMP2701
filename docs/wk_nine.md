# Week 9

## The `sizeof` Operator
C provides a unary (one-input) ***operator*** to determine an object's or type's
size in bytes (Bob stresses that `sizeof` is ***not*** a function).

It calculates the size of the object at *compile time* and not during runtime.

It always returns the number of bytes as a `size_t`.

When applied to arrays, `sizeof` will use the size of the underlying type times
the number of elements to determine the overall size of the array. For example,
```c
int array[20];
printf("%zu", sizeof array);
```
will output 80 (4*20) where 4 is the size of an int and 20 is the number of
elements in the array.

### Pointers and `sizeof`
One thing to watch out for is applying `sizeof` to pointers. Even if the pointer
is pointing to an array or some other type element, the sizeof operator will always
return the size of the *pointer* in the system and not the size of the element 
it points to. 

Recall that pointers are determined by the size of a register in the underlying
system / what the application expects the size of the register to be. In modern
systems this will be either 64- or 32-bit (8- or 4-bytes respectively).

This is especially important to remember when it comes to arrays. Remember that 
arrays *decay* to a pointer when they are passed to a function. In essence, 
once we are in the function, it only sees the pointer to the first element; the 
function will not know that the element pointed to is the start of an array.

### Sizes Caveat
Remember that, while type sizes are mostly consistent, they can *vary* on different 
systems. 

We can verify the sizes in our given system by 
using `sizeof` on a *type*. When we do this, we ***must*** use parentheses `()`
around the type: `sizeof(int)`.

## More on Pointers

### Pointer Arithmetic Operators
Pointers are valid operands in arithmetic operations (however, not all arithmetic
operators can be used with pointers).

The following arithmetic operators are allowed with pointers:
 
- incrementing `++` and decrementing `--`,
- adding an integer to a pointer `+` or `+=`,
- subtracting an integer from a pointer `-` or `-=`, and
- subtracting one pointer from another (to tell how far apart they are).

### Aiming a Pointer at an Array
We can point to an array as follows:
```c
int myArray[] = {0, 1, 2, 3, 4};

int *myPtr = myArray; // this will point to the first element of the array

//alternatively we can do the following equivalent expression
int *mySecondPtr = &myArray[0]; // go to myArray[0] and grab its address
```

### Adding (and subtracting) an Integer to (from) a Pointer
When you add an integer to a pointer, the compiler uses the size of the type 
to calculate the new address in blocks, not raw bytes.

For example, suppose you have a pointer `int *myPtr` pointing to address `0x1000`:

```nohighlight
+-----------+
|     0     |  <- Address 0x1000
+-----------+
|     1     |  <- Address 0x1004
+-----------+
|     2     |  <- Address 0x1008
+-----------+
|     3     |  <- Address 0x100C 
+-----------+
|     4     |  <- Address 0x1010 
+-----------+
```

If you added `2` to the raw address `0x1000` mathematically, you'd get `0x1002`.
<br>But when you write `myPtr += 2`, the compiler treats this as "move forward 2 ints," 
so it multiplies `2` by the size of an `int` (4 bytes). 
The result is `0x1000 + 8 = 0x1008`.

The same rule applies when subtracting an integer: `myPtr -= 1` moves the pointer
back by the size of one element.

This is also true for the increment `++` and decrement `--` operators: `myPtr++` 
moves to the next element, not just the next byte.

### Subtracting One Pointer from Another

You can subtract one pointer from another to find how many **elements** apart 
they are.

For example, suppose we have two pointers as follows:
```c
int *myPtr;      // assume it points to 0x1000
int *myOtherPtr; // assume it points to 0x1008.
```

These addresses are 8 bytes apart. Since each `int` is 4 bytes, `myOtherPtr - myPtr` 
we will get a result of 2. That is, these pointers are two *elements* apart.

The compiler automatically divides the byte difference by the size of the type,
so pointer subtraction always gives a **count of elements**, not raw bytes.

### Pointers to `void`
Pointers of the same type can be assigned to each other directly.

`void *` is a generic pointer type; *any* pointer can be assigned to a `void *`, 
and a `void *` can be assigned to any other pointer type without casting.

You can’t *dereference* a `void *` because the compiler doesn’t know what type 
(and size) it points to. You must cast it to a specific pointer type first.

## Arrays, Pointers, and Offset Notation

In C, an **array name** acts like a **constant pointer** to the array’s first element. For example, `b` is equivalent to `&b[0]`. This lets you assign a pointer to the start of an array like `bPtr = b;`.

Because of this, you can use **pointer arithmetic** instead of subscripts.  
For example:  
- `b[3]` is the same as `*(b + 3)`  
- `*(bPtr + 3)` is the same as `bPtr[3]`

The number added is an **offset**, which moves the pointer forward by that many
***elements***. Always use parentheses like `*(b + 3)`, because `*` has higher 
precedence than `+`. Without parentheses, `*b + 3` means "dereference `b` then 
add 3" instead of "move the pointer by 3 then dereference."

In general, any array element access `b[i]` can be written using **pointer/offset
notation**: `*(b + i)`.



