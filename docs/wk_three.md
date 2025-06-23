# Week 3

### Functions

A **function** is a self-contained block of code designed to accomplish a specific task. Functions enable:

- **Divide and conquer**: Developing large programs by combining smaller, reusable parts.
- **Abstraction**: Hiding implementation details behind a meaningful name allowing a developer to use a function without worrying about the underlying implementation.
- **Decomposition**: Breaking complex problems into manageable components - when a function can't be defined easily, we can decompose it into a series of smaller functions which are better defined. 

Programs are easier to write, test, debug, and maintain when structured as a collection of functions.

#### Functional Program Structure
In C, all programs must have a `main()` function. It serves as the entry point of execution. However, instead of doing everything itself we can *functionalize* the program (breaking it into individual functions) allowing `main()` to focus on the overall logic while calling other functions to do the actual work.

This setup reflects a hierarchical calling structure, where `main()` is the top-level caller (*boss*) and delegates tasks to helper functions (*workers*).

#### Function Definition
A function in `C` has the following definition:
```c
return_type function_name(parameter_list) {
    statements
}
```

The `return_type` tells the type returned from the function (`void` if there is no return value).
<br>The `function_name` is the identifier for this function, it should be a clear, easy-to-understand name.
<br>The `parameter_list` is a comma-separated list of input variables and their types
<br>The `statements` are the actions taken by the function. If there is a `return_type` other than `void` the function *must* have a `return` statement in it.

Example:
```c
int square(int number) {
    return number * number;
}
```

#### Function Prototype
Recall that `C` executes code sequentially. If it sees a *function caller* before the definition of that function, it won't know what to do and the compiler will give an error. We can get around this, somewhat, by placing functions in the order that they are being called but this *quickly* becomes a mess and often can't even be fully achieved. 

To get around this, we can include a *prototype* near the start of the program that tells the compiler what a function looks like before it's used.
```c
int square(int number);
```

This prototype allows the compiler to check that the function call has the right signature and also ensures that the return type is compatible for whatever the function is being called to do.

Prototypes *must* end in a semicolon `;` and do not have any of the statements listed. You can *optionally* omit the parameter names and only include types for the parameters
```c
int square(int);
```

**Note:** the idea of *prototypes* didn't always exist in `C`. It is actually borrowed from `C++`. Before this, the compiler wouldn't check the behaviour of functions meaning that no errors would be thrown at compilation but this led to more issues at runtime due to improper use of functions.

##### Argument Coercion
Function prototypes implicitly convert arguments to the appropriate type a process known as **argument coercion**. 
```c
printf("%.3f\n", sqrt(4)); // this works despite sqrt expecting a double.
//prints 2.000
```

We need to be careful with these coercions as if they are used incorrectly they can cause information to be lost. 

For example, if you pass a `double` to a function expecting an `int` the information after the decimal will be dropped. If that information was valuable this can lead to incorrect results. 

Furthermore, if a function expects a small type such as an `short` but we pass it a larger type such as a `long` we risk changing the value as any extra bits will be dropped from the `long`.

###### Mixed-Type Expressions
When we perform an operation which has multiple types, the compiler will make temporary copies of all the values to the *highest* type used: a process known as **promotion**. For example, if we have a `double` and a `float` everything is converted to a `double` because it is the higher size type.

### Function-Call Stack
A **stack** is a common data-structure. It is often compared with a pile of plates. Fresh plates are placed on the top (known as being *pushed* onto the stack) and when they are removed we also grab from the top (and *pop* off the stack).

Stacks are ***last-in, first-out (LIFO)***.

The *function-call stack* is what supports the underlying function call and return for a program. When we call a function, we might call other functions. Each function needs to remember who to return its value to. The *function-call stack* makes this easy.

Each time a function calls another function, we push a **stack frame* containing the *return address* for the new function to return to the old function. Once a function returns, the associated stack frame is popped and returns control to the return address that is specified in that stack frame. Because it is a stack, each function will *always* find its return address at the top of the stack, once it is used it is popped off the stack so the next function sees the correct address.

If we push too many functions to the stack, we will eventually run out of memory and will have a *stack overflow* error crashing the program.


### Pass Arguments by Value and by Reference
In programming, there are two ways to pass an argument into a function: *pass by value* and *pass by reference*.

**Passing by value** means that we **copy** the value of an argument into the function. Since the function then works on a copy, it does *not* affect the original variable's value. 

```c
int c = 5;
superFunction(c);           // will print 5000;
printf("outside: %d\n", c); // will print 5

void superFunction(int num){
	num *= 1000;
	printf("inside: %d\n", num);
	return 
}
```

**Passing by reference** means that we pass the actual variable (or at least a reference to it) into the function. This means that the function *does* modify the original variable's value as a result.

In `C` all arguments are passed by *value*. With that said, we can use pointers to achieve pass by reference functionality. 


### Random-Number Generation
In `C` the standard library `<stdlib.h>` has a few useful functions that we can use to generate random numbers. In this course we will look at `rand()` and `srand()`.

To create a random value we can simply use `rand()` as follows: `int value = rand();`. This will generate a random value between 0 and `RAND_MAX` (which is a symbolic constant defined in `<stdlib.h>` and is *at least* 32,767 but can be higher depending on system/compiler).
    <br> Note: even though its defined as at least 32,767, the value is *always* of type `int` (or more clearly `signed int`) and not `short`. 

We often want to limit the range of possible values returned when we generate a random number. For example, if we wanted to simulate a dice roll, we would only want `1, 2, 3, 4, 5, 6` as our possible values but as we see `rand()` return between 0 and `RAND_MAX` which is way more numbers than we want. To get around this, we can use the **modulo** operator and work just with remainders. 
```c
int diceValue = 1 + (rand() % 6) // note: rand() % 6 will return a number between 0 and 5. We add 1 to *shift* the result to 1 through 6.
```
This is known as **scaling**. In our example above, 6 is our **scaling factor** and adding one is our **shift**.

#### Seeds and `srand()`
Each subsequent call to `rand()` calculates a new random number based on the previous result. This works within a single execution of the program, however if we run the program again we will discover that it returns the *exact* same results the next time it is run. This is because `rand()` starts with a default **seed** or starting value (usually just 1). 

To get randomized results for every execution, we can use the `srand()` function to ***s****eed* `rand()`.  Note that this function just changes the seed value that will be used by `rand()`. After seeding, we still use the regular `rand()` function to generate our random numbers. That is, we only run `srand()` once to set things up. We can pass any number to `srand()` but if we hardcode that number in the program we will end up with the same issue, namely that every execution will start with the same seed and thus have the same results. 

##### `time(NULL)`
To get around this, we can use `time(NULL)` as our seed: `srand(time(NULL));`. `time(NULL)` returns the number of seconds that have passed since midnight `Jan 1, 1970` and therefore is changing every second. 

#### Secure Random Number Generation
The standard C `rand()` function is fine for textbook examples but is not suitable for secure or industrial-strength applications.

According to the C standard:
> "There are no guarantees as to the quality of the random sequence produced, and some implementations are known to produce sequences with distressingly non-random low-order bits."

##### Secure Alternatives for Production
For cryptography or any security-critical task, `rand()` is ***not*** safe. Instead, use platform-specific secure random-number generators:

| Platform      | Recommended Function | Notes                                                                                                                    |
| ------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Windows       | `BCryptGenRandom`    | Part of the Cryptography API: Next Generation ([docs](https://docs.microsoft.com/en-us/windows/win32/seccng/cng-portal)) |
| Linux / POSIX | `random`             | View details with `man random` in terminal                                                                               |
| macOS         | `arc4random`         | View details with `man arc4random` in terminal                                                                           |

Refer to the CERT Secure Coding Guideline:  
[MSC30-C: Do not use the rand() function for generating pseudorandom numbers](https://wiki.sei.cmu.edu/confluence/display/c/MSC30-C)

Always use secure generators when random numbers must not be predictable.


