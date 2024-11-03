# COMP10002 Reference Sheet

- [Bitwise operators](#bitwise-operators)
- [Base conversion](#base-conversion)
- [Format specifiers](#format-specifiers)
- [Static variables](#static-variables)
- [Void pointers](#void-pointers)
- [stdlib.h](#stdlibh)
- [stdio.h](#stdioh)
- [assert.h](#asserth)
- [ctype.h](#ctypeh)
- [string.h](#stringh)
- [Alogrithms](#algorithms)
- [Data structures](#data-structures)

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
    - Generates a pseudo-random integer between 0 and RAND_MAX (a constant defined in <stdlib.h>, typically 32767).
- `rand() % n`
    - Limits the output of `rand()` to a specific range, from `0` to `n-1`
- `srand(k)`
    - Initialises the `rand()` function with a seed `k`
- `srand(time(NULL))`
    - Seeds the random number generator with the current time, making the sequence of random numbers different each time the program is run.
    - Requires `<time.h>`
- `abs(int value)`
    - Only works on `int` values
- `atof(const char * str)`
    - Returns: A `double` value containing the number represented by the string.
- `atoi(const char * str)`
    - Returns: An `int` value containing the number represented by the string.
- `qsort(void * arr, size_t amount, size_t size, compare)`
    - `arr`: Required. Specifies the array to be sorted.
    - `amount`: Required. Specifies the number of elements in the array.
    - `size`: Required. Specifies the size of an element in the array measured in bytes.
    - `compare`: Required. Specifies the size of an element in the array measured in bytes.
        -  The function should have the structure `int myFunction(const void * a, const void * b)` where the parameters `a` and `b` are pointers to elements in the array being compared. The function should return a positive number if `a > b`, a negative number if `a < b` and zero if `a == b`.
- `malloc(size_t size)`
    - Returns: A `void *` pointer to the newly allocated block of memory.
    - Ex: `int *myArray = malloc(numItems * sizeof(int));`
- `realloc(void * ptr, size_t size)`
    - Returns: A `void *` pointer to the block of memory.
    - Ex: `myArray = realloc(myArray, 2 * numItems * sizeof(int));`
- `free(void * ptr)`
    - Ex: `free(myArray)`
 
## stdio.h

- `getchar()`
    - Returns: An `int` value representing the ASCII value of the character that was read.
- `putchar(int char)`
    - Returns: An `int` value representing the ASCII value of the character that was written.
- `fopen(const char * filename, const char * mode)`
    - Returns: A `FILE` pointer which can be used by other file handling functions.
    - Create a file: `fptr = fopen("filename.txt", "w");`
- Modes
    - `"w"`: Open for writing only. Clears all of the content of the file. If the file does not exist it will be created.
    - `"a"`: Open for writing only. Only writes to the end of the file. If the file does not exist it will be created.
    - `"r"`: Open for reading only. If the file does not exist then a NULL pointer is returned.
- `fclose(FILE * fptr)`
    - Returns: An `int` value which is `0` if the file was closed successfully. A different value indicates that an error occurred.
- `fread(void * destination, size_t size, size_t amount, FILE * fptr)`
    - `destination`: Required. A pointer to a block of memory where the data will be written.
    - `size`: Required. The size of an element in the block of memory.
    - `amount`: Required. The number of elements to read from the file and write into the block of memory.
    - `fptr`: Required. A file pointer, usually created by the `fopen()` function.
    - Returns: A `size_t` value representing the number of elements that were read. If this number is different than the amount parameter then the end of the file has been reached or an error occurred.
- `fwrite(const void * source, size_t size, size_t amount, FILE * fptr)`
    - `source`: Required. A pointer to a block of memory where the data is copied from.
    - `size`: Required. The size of an element in the block of memory.
    - `amount`: Required. The number of elements to read from the block of memory and write into the file. 
    - Returns: A `size_t` value representing the number of elements that were written into the file.
- `fscanf(FILE * fptr, const char * format, arg1, arg2...)`
    - Returns: An `int` value representing the number of arguments that were written to. It returns the constant `EOF` if an error occurred.
    - Ex:
```
char output[50];
fscanf(fptr, "%49s", output);
printf("%s", output);
```
- `fprintf(FILE * fptr, const char * format, arg1, arg2...)`
    - Returns: An `int` value representing the number of characters written to the file. If an error occurred then it returns a negative number.
    - Ex: `fprintf(fptr, "Some text");`
- `fgets(char * destination, int size, FILE * fptr)`
    - `destination`: Required. A pointer to the array where the content will be written.
    - `size`: Required. The size of the array being written to. The function will read at most size-1 characters from the file. 
    - Returns: The same pointer that was provided by the `destination` parameter.
- `fseek(FILE * fptr, long int offset, int origin)`
    - `offset`: Required. Specifies a position in the file relative to `origin`.
    - `origin`: Required. Specifies the position in the file from which the `offset` is applied. It can be one of the following constants:
        - SEEK_SET - Offset is relative to the beginning of the file
        - SEEK_CUR - Offset is relative to the current position in the file
        - SEEK_END - Offset is relative to the end of the file (SEEK_END value may not be fully supported by some implementations of the library.)
    - Returns: An `int` value which is zero if successful and non-zero if an error occurred.

## assert.h

- `assert(expression)`

## ctype.h

- `int isalnum(int ch)`
    - Returns: A non-zero `int` if the character is alphanumeric. `0` otherwise.
- `int isalpha(int ch)`
    - Returns: A non-zero `int` if the character is a letter. `0` otherwise.
- `int isdigit(int ch)`
    - Returns: A non-zero `int` if the character is a decimal digit. `0` otherwise.
- `int islower(int ch)`
    - Returns: A non-zero `int` if the character is a lowercase letter. `0` otherwise.
- `int isupper(int ch)`
    - Returns: A non-zero `int` if the character is an uppercase letter. `0` otherwise.
- `int tolower(int ch)`
    - Returns: An `int` value of the ASCII value of the lowercase version of a character.
- `int toupper(int ch)`
    - Returns: An `int` value of the ASCII value of the uppercase version of a character.

## string.h

- `void *memcpy(void *dest_str, const void * src_str, size_t n`
    - Returns: pointer to `dest_str`
- `strcat(void * destination, void * source)`
    - Returns: A `char` type pointer to the destination string.
- `strcmp(const char * str1, const char * str2)`
    - Returns: `int` value. Positive if `str1 > str2`, negative if `str1 < str2`, zero if `str1 == str2`.
- `strcpy(char * destination, char * source)`
    - Returns: A `char` type pointer to the destination string.
- `strlen(char * str)`
    - Returns: `int`

## Algorithms

### BMH

### Burrows-Wheeler transform

### KMP

### BMH

### Quicksort

### Insertion sort

### Selection sort

### Heapsort

### Merge sort

## Data structures

### Binary search trees

### Heaps

### Linked lists
