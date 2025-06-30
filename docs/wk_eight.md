# Week 8

## Pointers
Pointers are variables whose values are memory addresses. 

Refercing a value through a pointer is called **indirection** because we are 
using the address to find the value rather than directly accessing the value.

<details><summary>More on <strong>Indirection</strong></summary>
<hr>
To understand this concept lets imagine a block of memory as follows:

```nohighlight
+-----------+
|    ???    |  <- Address 0x1000
+-----------+
|    ???    |  <- Address 0x1004
+-----------+
|    ???    |  <- Address 0x1008
+-----------+
|    ???    |  <- Address 0x100C 
+-----------+
```

Suppose we create a variable <code>count</code> and assign it the value of 7.
```c
int count = 7;
```

The program updates memory to place the value 7 at the first available address.

```nohighlight
+-----------+
|     7     |  <- Address 0x1000 (count)
+-----------+
|    ???    |  <- Address 0x1004
+-----------+
|    ???    |  <- Address 0x1008
+-----------+
|    ???    |  <- Address 0x100C 
+-----------+
```

Next we create a pointer to that memory.
```c
int *countPtr = &count;
```

Our memory would now look something like this.

```nohighlight
+-----------+
|     7     |  <- Address 0x1000 (count)
+-----------+
|    ???    |  <- Address 0x1004
+-----------+
|    ???    |  <- Address 0x1008
+-----------+
|   0x1000  |  <- Address 0x100C (countPtr)
+-----------+
```

The variable <code>countPtr</code> holds the value <code>0x1000</code> which is the address of count.

<br>The original variable, <code>count</code> directly references the value <code>7</code>.

<br><code>countPtr</code> on the other hand, <strong>indirectly</strong> references <code>7</code>.
It tells you where to find the variable that holds the value <code>7</code>.
<hr>
</details>

<br>

### Declaring a Pointer
To distinguish between a pointer and a regular variable, we use `*` to indicate
to the compiler that the variable is a pointer.
```c
int *countPtr;
```

This is read as "countPtr is a pointer to an int".

It can be helpful to name a pointer in a way that makes it clear it is a pointer
to the programmer such as ending the variable name with `Ptr`.

#### Declaring Multiple Variables
If you are declaring multiple variables, it’s best to split them over multiple lines.
<br>For example, consider the following code which creates one pointer and one regular variable.

```c
int *countPtr, count;
``` 

This can be ambiguous, especially if you use the alternate syntax: 
```c
int* countPtr, count;
``` 
This makes it look like both variables would be `int*`.

To avoid this ambiguity, its often best to just split the declarations over multiple lines.
```c
int* countPtr;
int count;
```

### Initializing a Pointer
When we define a pointer we should always initialize it with some value, even if
that value is `NULL`. 
```c
int *countPtr = NULL;
```

Note we can also use `0` in place of `NULL` since `NULL` refers to address `0`. 

```c
int *countPtr = 0;
```

However, in practice it is best to be explicit about it being `NULL`.

### The Address (`&`) Operator
The unary *address operator* `&` returns the address of its operand.

```c
int y = 5;
int *yPtr = &y;
```

We use the `&y` to assign the ***address*** of `y` to `yPtr`.

### The Indirection/Dereferencing (`*`) Operator
The *derefercing operator* `*` allows us to follow a pointer to access the value that 
it points to.
```c
printf("%d", *yPtr); // prints 5
```

#### Note: Dual Meanings of `*`
In C `*` has different meanings depending on context:

 - In a ***declaration***, it means the variable is a pointer to the given type.
 - In an expression, it means ***dereference*** the pointer (access the value at the address)

