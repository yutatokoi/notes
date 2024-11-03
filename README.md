# COMP10002 Reference Sheet

## Bitwise operators

`&`: AND
`|`: OR
`^`: XOR
`~`: NOT
`<<`: Left shift
`>>`: Right shift

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
