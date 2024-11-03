# COMP10002 Reference Sheet

- [Bitwise operators](#bitwise-operators)

## Bitwise operators

- `&`: AND
- `|`: OR
- `^`: XOR
- `~`: NOT
- `<<`: Left shift
- `>>`: Right shift

```
unsigned int b = 9;
// The result is 00010010
printf("b<<1 = %u\n", b << 1);

// The result is 00000100
printf("b>>1 = %u\n", b >> 1);
```

## Base conversion

```
// Convert num to hexadecimal string, store it in buffer
sprintf(buffer, "%x", num);

// Convert num to octal string, store it in buffer
sprintf(buffer, "%o", num);
```

## Format specifiers

- `printf("%5d", 42);`
    - Right aligned, using minimum of 5 spaces
- `printf("%*d", 5, 42);`
    - Right aligned. Width is specified by argument corresponding to `*`
- `printf("%-5d", 42);`
    - Left-justified, using minimum of 5 spaces
- `printf("%.2f", 3.14159);`
    - Output: `3.14`
- `printf("%.3s", "Hello");`
    - Output: `Hel`
- `printf("%04d", 42);`
    - Output: `0042`
- `printf("%8.2f", 3.14159);`
    - Output: `    3.14`
    - (total width is 8, with 2 decimal places and 4 space padding, right aligned)

## Static variables

`static int count = 0;`

These variables are defined within a function and retain their value between calls to that function.

## Void pointers

`void *ptr;`

A special type of pointer that does not have an associated data type. This allows it to point to any type of data without needing to be cast explicitly to match a specific type during assignment.

```
#include <stdio.h>

void printValue(void *ptr, char type) {
    if (type == 'i') {
        printf("Integer: %d\n", *(int *)ptr);
    } else if (type == 'f') {
        printf("Float: %.2f\n", *(float *)ptr);
    } else if (type == 'c') {
        printf("Character: %c\n", *(char *)ptr);
    }
}

int main() {
    int num = 42;
    float pi = 3.14;
    char letter = 'A';

    printValue(&num, 'i');
    printValue(&pi, 'f');
    printValue(&letter, 'c');

    return 0;
}
```

To dereference a void pointer, you must cast it to the appropriate type first:

```
void *ptr;
int a = 5;
ptr = &a;  // Assign the address of an int variable to the void pointer

// Cast to int pointer to dereference
printf("Value: %d\n", *(int *)ptr);
```

## stdlib.h

- `rand()`
    - 
- `rand() % n`
    - 
- `srand(k)`
    - initialises the `rand()` function with a seed `k`
- `srand(time(NULL))`
    - a good way to start the program before calling `rand()`

## Algorithms

BMH

Burrows-Wheeler Transform

QuickSort

Insertion Sort

Selection Sort

Heap Sort

Merge Sort

## Data Structures

Binary Search Trees

Heaps

Linked Lists
