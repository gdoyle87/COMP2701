# Week 2

## Algorithms
**Definition:** An algorithm is a procedure for solving a problem, defined by:

1. The actions to execute.
2. The order in which these actions execute.

The order of statement execution is called program control.

## Control Structures
By default, C programs execute statements in the order they are written (sequential execution). However, control structures allow transferring control to execute statements out of sequence. There are three types:

1. Sequence Structure: Statements execute in the order written.
2. Selection Structures: if, if...else, and switch choose the next statement based on a condition.
3. Iteration Structures: for, while, and do...while repeat statements until a condition is met.

### Selection Structures
#### `if` and `if...else` Statements
The if statement evaluates a condition and executes a block if true. An optional else block executes if false.

Example:
```c
int x = 5;
if (x > 0) {
    printf("x is positive\n");
} else {
    printf("x is non-positive\n");
}
// Output: x is positive
``` 

##### Ternary Conditional Operator
The ternary operator (`?:`) is a concise alternative to `if...else` for simple conditions. It returns the value after ? if the condition is true, or the value after : if false.
```c
int x = 5;
puts(x > 0 ? "good" : "bad");
// Output: good
```

**Note:** Use the ternary operator for simple expressions but probably best to avoid for more complex expressions as it can reduce readability for complex logic.


##### Short-Circuit Evaluation
In `C`, logical operators (`&&` and `||`) use *short-circuit* evaluation. If the result of an expression can be determined early, the remaining parts are not evaluated.

For `&&` (**AND**): If the first condition is *false*, the second is not evaluated.
<br>For `||` (**OR**): If the first condition is *true*, the second is not evaluated.

Example:
```c
int x = 0, y = 5;
if (x != 0 && y / x > 2) { // y / x not evaluated due to x == 0
    printf("This won't print\n");
}
```

This prevents errors like division by zero.

#### `switch()` Statement
`switch` provides a clean way to branch based on the value of a single variable 
(usually `int` or `char`). It’s often used instead of long `if...else if` chains.

```c
int grade = 2;
switch (grade) {
    case 1:
        puts("Excellent");
        break;
    case 2:
        puts("Good");
        break;
    case 3:
        puts("Fair");
        break;
    default:
        puts("Invalid");
}
```

If we don't put a `break;` after a case execution will fall through to the next case.
Sometimes this is intentional when multiple values will have the same response. Other times,
this is a bug and will lead to unexpected behaviour.
```c
case 1: // Fall-through intentional
case 2:
    puts("Grade is 1 or 2");
    break;
```


### Iteration Structures
#### `while` Loop
The `while` loop repeats as long as a condition is true, checking the condition before each iteration.
```c
int i = 3;
while (i < 10) {
    printf("%d ", i);
    i++;
}
// Output: 3 4 5 6 7 8 9
```

#### `do...while` Loop
The `do...while` loop executes at least once, checking the condition after each iteration.
```c
int i = 3;
do {
    printf("%d ", i);
    i++;
} while (i < 4);
// Output: 3
```

#### `for` Loop
The `for` loop is used for iterations with a known number of repetitions, combining initialization, condition, and increment.
```c
for (int i = 0; i < 5; i++) {
    printf("%d ", i);
}
// Output: 0 1 2 3 4
```

#### Pre-Increment vs. Post-Increment

**Pre-increment (`++i`):** Increments `i` before using its value.
<br>**Post-increment (`i++`):** Uses `i`’s value, then increments it.

Example:
```c
int i = 0;
printf("%d\n", i++); // Prints 0, then i becomes 1
printf("%d\n", ++i); // Increments i to 2, then prints 2
```

<br>*Bob's Preference:* Use pre-increment (`++i`) in loops for slight performance benefits (avoids creating a temporary copy) and clarity.

In a `for` loop, the increment step (e.g., `i++` or `++i` in 
`for (int i = 0; i < 5; i++)`) occurs after the loop body executes, 
so the choice of pre- or post-increment doesn’t affect the loop’s iteration behavior. 
Both produce the same sequence of loop iterations.
<br>However, within the loop body, the choice matters if the incremented 
value is used. For example, if you use `i++` or `++i` to modify another 
variable, the result differs.

#### Loop Exit Conditions
When using different increment types or loop structures, carefully consider when the loop exits:

**Integer increments:** Ensure the condition accounts for the final value.
```c
int i = 0;
while (i < 5) {
    printf("%d ", i);
    i += 2; // Increments by 2
}
// Output: 0 2 4
```


**Floating-point increments:** Be cautious with floating-point conditions due to precision issues (basically, *never* do this).
```c
float i = 0.0;
while (i < 1.0) {
    printf("%.1f ", i);
    i += 0.1;
}
// May not stop exactly at 1.0 due to floating-point imprecision
```


#### Loop type choice:
Use `while` for condition-driven loops.
<br>Use `do...while` when *at least* one iteration is needed.
<br>Use `for` for counted loops with clear start and end points.



## Casting Numbers
When dividing two integers, the result is an integer, truncating any decimal part. 
To retain decimals, cast one or both operands to `float` or `double`.
```c
int a = 2, b = 3;
float result = (float)a / b; // Cast a to float
printf("%.2f\n", result); // %.2f formats to 2 decimal places
// Output: 0.67
```

**Note:** Use `%.nf` in `printf` to specify n decimal places (e.g., `%.2f` for two decimals).

## Arithmetic Overflow <limits.h>
The `<limits.h>` header provides constraints for various types based on the underlying system limits.

Example:
```c
#include <limits.h>
#include <stdio.h>
int main(void) {
    int x = INT_MAX;
    if (x > INT_MAX - 1) { // Check for overflow
		printf("Cannot increment without overflow\n");
    } else {
		x++;
    }
    return 0;
}
```
