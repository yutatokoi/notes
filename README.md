# COMP10002 Reference Sheet

- [C](#c)
    - [Bitwise operators](#bitwise-operators)
    - [Base conversion](#base-conversion)
    - [Format specifiers](#format-specifiers)
    - [Static variables](#static-variables)
    - [Void pointers](#void-pointers)
- [Standard library](#standard-library)
    - [stdlib.h](#stdlibh)
    - [stdio.h](#stdioh)
    - [assert.h](#asserth)
    - [ctype.h](#ctypeh)
    - [string.h](#stringh)
    - [math.h](#mathh)
- [Alogrithms](#algorithms)
    - [BMH](#bmh)
    - [KMP](#kmp)
    - [Burrows-Wheeler transform](#burrows-wheeler-transform)
    - [Quicksort](#quicksort)
    - [Quicksort on linked list](#quicksort-on-linked-list)
    - [Insertion sort](#insertion-sort)
    - [Bubble sort](#bubble-sort)
    - [Selection sort](#selection-sort)
    - [Heapsort](#heapsort)
    - [Merge sort](#merge-sort)
    - [Merge sort on linked list](#merge-sort-on-linked-list)
- [Data structures](#data-structures)
    - [Binary search trees](#binary-search-trees)
    - [Heaps](#heaps)
    - [Priority queues](#priority-queues)
    - [Linked lists](#linked-lists)
    - [Dictionaries](#dictionaries)
- [Miscellaneous](#miscellaneous)

## C

### Bitwise operators

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

### Base conversion

```
// Convert num to hexadecimal string, store it in buffer
sprintf(buffer, "%x", num);

// Convert num to octal string, store it in buffer
sprintf(buffer, "%o", num);
```

### Format specifiers

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

### Static variables

`static int count = 0;`

These variables are defined within a function and retain their value between calls to that function.

### Void pointers

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

To dereference a void pointer, it must be cast to the appropriate type first:

```
void *ptr;
int a = 5;
ptr = &a;  // Assign the address of an int variable to the void pointer

// Cast to int pointer to dereference
printf("Value: %d\n", *(int *)ptr);
```

## Standard library

### stdlib.h

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
 
### stdio.h

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

### assert.h

- `assert(expression)`

### ctype.h

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
- `int toupper(int ch)`

### string.h

- `void *memcpy(void *dest_str, const void * src_str, size_t n`
    - Returns: pointer to `dest_str`
- `strcat(void * destination, void * source)`
    - Returns: A `char` type pointer to the destination string.
- `strcmp(const char * str1, const char * str2)`
    - Returns: `int` value. Positive if `str1 > str2`, negative if `str1 < str2`, zero if `str1 == str2`.
- `strcpy(char * destination, char * source)`
    - Returns: A `char` type pointer to the destination string.
- `int strlen(char * str)`
 
### math.h

- `double ceil(double number)`
- `double floor(double number)`
- `double round(double number)`
- `double trunc(doulbe number)`
- `double fabs(double number)`
- `double log(double number)`
    - Natural logarithm
- `double log10(double number)`
    - Base 10 logarithm
- `double log2(double number)`
    - Base 2 logarithm
- `double pow(double base, double exponent)`
- `double sqrt(double number)`

## Algorithms

### BMH

Substring searching algorithm.

Performance
- Average case run-time: O(n)
- Worst case run-time: O(nm), where pattern is length m and length of search string is n

```
#define ALPHABET_SIZE 256 // Assuming extended ASCII

void preprocessBadCharacterTable(char *pattern, int patternLength, int badCharTable[]) {
    for (int i = 0; i < ALPHABET_SIZE; i++) {
        badCharTable[i] = patternLength; // Default shift length is pattern length
    }
    for (int i = 0; i < patternLength - 1; i++) {
        badCharTable[(unsigned char)pattern[i]] = patternLength - 1 - i; // Shift based on position
    }
}

int BMHSearch(char *text, char *pattern) {
    int textLength = strlen(text);
    int patternLength = strlen(pattern);
    int badCharTable[ALPHABET_SIZE];

    preprocessBadCharacterTable(pattern, patternLength, badCharTable);

    int offset = 0;
    while (offset <= textLength - patternLength) {
        int j = patternLength - 1;
        while (j >= 0 && pattern[j] == text[offset + j]) {
            j--;
        }
        if (j < 0) {
            return offset; // Match found
        }
        offset += badCharTable[(unsigned char)text[offset + patternLength - 1]];
    }
    return -1; // No match found
}
```

### KMP

Both BMH and KMP are not ideal for short strings due to the minimal benefits that the preprocessing provides.

Performance
- Space complexity: O(m)
- Worse case run-time: O(m + n)

```
// Function to build the LPS (Longest Prefix Suffix) array
void buildLPSArray(char *pattern, int patternLength, int *LPS) {
    int length = 0; // Length of the previous longest prefix suffix
    LPS[0] = 0;     // LPS[0] is always 0

    int i = 1;
    while (i < patternLength) {
        if (pattern[i] == pattern[length]) {
            length++;
            LPS[i] = length;
            i++;
        } else {
            if (length != 0) {
                length = LPS[length - 1];
            } else {
                LPS[i] = 0;
                i++;
            }
        }
    }
}

// Function to perform KMP search
void KMPSearch(char *text, char *pattern) {
    int textLength = strlen(text);
    int patternLength = strlen(pattern);

    // Create the LPS array
    int LPS[patternLength];
    buildLPSArray(pattern, patternLength, LPS);

    int i = 0; // Index for text
    int j = 0; // Index for pattern
    while (i < textLength) {
        if (pattern[j] == text[i]) {
            j++;
            i++;
        }

        if (j == patternLength) {
            printf("Pattern found at index %d\n", i - j);
            j = LPS[j - 1]; // Move pattern based on LPS array
        } else if (i < textLength && pattern[j] != text[i]) {
            if (j != 0) {
                j = LPS[j - 1]; // Shift pattern based on LPS array
            } else {
                i++;
            }
        }
    }
}
```

### Burrows-Wheeler transform

(Not assessed for the final exam).

Preprocessing for lossless compression on strings. Used in bzip2.

Performance
- Worst case run-time: O(n)
- Space complexity: O(n)

### Quicksort

Performance
- Worst case run-time: O(n^2)
- Average run-time: O(nlog(n))
- Best case run-time: O(n)

Most implementations are not stable.

```Pseudocode
// Sorts a (portion of an) array, divides it into partitions, then sorts those
algorithm quicksort(A, lo, hi) is 
  // Ensure indices are in correct order
  if lo >= hi || lo < 0 then 
    return
    
  // Partition array and get the pivot index
  p := partition(A, lo, hi) 
      
  // Sort the two partitions
  quicksort(A, lo, p - 1) // Left side of pivot
  quicksort(A, p + 1, hi) // Right side of pivot

// Divides array into two partitions
algorithm partition(A, lo, hi) is 
  pivot := A[hi] // Choose the last element as the pivot

  // Temporary pivot index
  i := lo

  for j := lo to hi - 1 do 
    // If the current element is less than or equal to the pivot
    if A[j] <= pivot then 
      // Swap the current element with the element at the temporary pivot index
      swap A[i] with A[j]
      // Move the temporary pivot index forward
      i := i + 1

  // Swap the pivot with the last element
  swap A[i] with A[hi]
  return i // the pivot index
```

### Quicksort on linked list

Quicksort is a stable sort when implemented on a linked list. It also does not require auxilliary memory space as it simply manipulates pointers.

```
// Function to perform Quicksort on a linked list
Node* quickSort(Node* head) {
    // Base case: 0 or 1 element
    if (head == NULL || head->next == NULL) {
        return head;
    }

    // Choose pivot (we'll use the head node)
    int pivot = head->data;

    // Initialize pointers for three sublists
    Node* lessHead = NULL;
    Node* lessTail = NULL;
    Node* equalHead = NULL;
    Node* equalTail = NULL;
    Node* greaterHead = NULL;
    Node* greaterTail = NULL;

    // Partitioning step
    Node* current = head;
    while (current != NULL) {
        if (current->data < pivot) {
            // Append to 'less' list
            appendNode(&lessHead, &lessTail, current);
        } else if (current->data == pivot) {
            // Append to 'equal' list
            appendNode(&equalHead, &equalTail, current);
        } else {
            // Append to 'greater' list
            appendNode(&greaterHead, &greaterTail, current);
        }
        current = current->next;
    }

    // Terminate the sublists
    if (lessTail != NULL) lessTail->next = NULL;
    if (equalTail != NULL) equalTail->next = NULL;
    if (greaterTail != NULL) greaterTail->next = NULL;

    // Recursively sort 'less' and 'greater' lists
    Node* sortedLess = quickSort(lessHead);
    Node* sortedGreater = quickSort(greaterHead);

    // Concatenate the lists: sortedLess + equal + sortedGreater
    return concatenateLists(sortedLess, equalHead, sortedGreater);
}

// Helper function to append a node to the end of a list
void appendNode(Node** headRef, Node** tailRef, Node* node) {
    if (*headRef == NULL) {
        // Empty list
        *headRef = node;
        *tailRef = node;
    } else {
        // Non-empty list
        (*tailRef)->next = node;
        *tailRef = node;
    }
}

// Helper function to concatenate three lists
Node* concatenateLists(Node* less, Node* equal, Node* greater) {
    Node* head = NULL;
    Node* tail = NULL;

    // Add 'less' list
    if (less != NULL) {
        head = less;
        tail = getTail(less);
    }

    // Add 'equal' list
    if (equal != NULL) {
        if (head == NULL) {
            head = equal;
            tail = getTail(equal);
        } else {
            tail->next = equal;
            tail = getTail(equal);
        }
    }

    // Add 'greater' list
    if (greater != NULL) {
        if (head == NULL) {
            head = greater;
        } else {
            tail->next = greater;
        }
    }

    return head;
}

// Helper function to get the tail of a list
Node* getTail(Node* head) {
    while (head != NULL && head->next != NULL) {
        head = head->next;
    }
    return head;
}
```

### Insertion sort

Stable algorithm that is very simple.

Performance:
- Worst case run-time: O(n^2)
- Average case run-time: O(n^2)
- Becase case run-time: O(n)
- Space complexity: O(1)

```Pseudocode
i ← 1
while i < length(A)
    j ← i
    while j > 0 and A[j-1] > A[j]
        swap A[j] and A[j-1]
        j ← j - 1
    end while
    i ← i + 1
end while
```

### Bubble sort

Performance:
- Worst case run-time: O(n^2)
- Average case run-time: O(n^2)
- Best case run-time: O(n)
- Space complexity: O(1)

```Pseudocode
procedure bubbleSort(A : list of sortable items)
    n := length(A)
    repeat
        swapped := false
        for i := 1 to n-1 inclusive do
            { if this pair is out of order }
            if A[i-1] > A[i] then
                { swap them and remember something changed }
                swap(A[i-1], A[i])
                swapped := true
            end if
        end for
    until not swapped
end procedure
```

### Selection sort

Performance
- Worst case run-time: O(n^2)
- Average case run-time: O(n^2)
- Best case run-time: O(n^2)
- Space complexity: O(1)

```Example run through
arr[] = 64 25 12 22 11

// Find the minimum element in arr[0...4]
// and place it at beginning
11 25 12 22 64

// Find the minimum element in arr[1...4]
// and place it at beginning of arr[1...4]
11 12 25 22 64

// Find the minimum element in arr[2...4]
// and place it at beginning of arr[2...4]
11 12 22 25 64

// Find the minimum element in arr[3...4]
// and place it at beginning of arr[3...4]
11 12 22 25 64
```

### Heapsort

In-place sorting. Unstable sort.

Performance
- Worst case run-time: O(nlog(n))
- Average case run-time: O(nlog(n))
- Best case run-time: O(nlog(n))
- Space complexity: O(1)

1. Heapfiy the input
2. Repeatedly extract the largest/smallest element and swap with the last element in the array

### Merge sort

Performance
- Worst case run-time: O(nlog(n))
- Average case run-time: O(nlog(n))
- Best case run-time: O(nlog(n))
- Space compexity: O(n)

1. Divide the unsorted list into `n` sublists, each containing one element (a list of one element is considered sorted).
2. Repeatedly merge sublists to produce new sorted sublists until there is only one sublist remaining. This will be the sorted list.

```Pseudocode
function merge_sort(list m) is
    // Base case. A list of zero or one elements is sorted, by definition.
    if length of m ≤ 1 then
        return m

    // Recursive case. First, divide the list into equal-sized sublists
    // consisting of the first half and second half of the list.
    // This assumes lists start at index 0.
    var left := empty list
    var right := empty list
    for each x with index i in m do
        if i < (length of m)/2 then
            add x to left
        else
            add x to right

    // Recursively sort both sublists.
    left := merge_sort(left)
    right := merge_sort(right)

    // Then merge the now-sorted sublists.
    return merge(left, right)
```

### Merge sort on linked list

For sorting linked lists, merge sort is typically a better choice than quicksort due to its synergy with linked lists. Quicksort is more suited for arrays.

```
// Function to perform merge sort on a linked list
Node* mergeSort(Node* head) {
    // Base case: if head is NULL or only one element
    if (head == NULL || head->next == NULL) {
        return head;
    }

    // Get the middle of the list
    Node* middle = getMiddle(head);
    Node* nextOfMiddle = middle->next;

    // Split the list into two halves
    middle->next = NULL;

    // Recursively sort the two halves
    Node* left = mergeSort(head);
    Node* right = mergeSort(nextOfMiddle);

    // Merge the sorted halves
    Node* sortedList = sortedMerge(left, right);

    return sortedList;
}

// Function to merge two sorted linked lists
Node* sortedMerge(Node* a, Node* b) {
    Node* result = NULL;

    // Base cases
    if (a == NULL) return b;
    if (b == NULL) return a;

    // Pick either a or b, and recur
    if (a->data <= b->data) {
        result = a;
        result->next = sortedMerge(a->next, b);
    } else {
        result = b;
        result->next = sortedMerge(a, b->next);
    }

    return result;
}

// Function to find the middle of the linked list
Node* getMiddle(Node* head) {
    if (head == NULL) return head;

    Node* slow = head;
    Node* fast = head->next;

    // Move fast by two nodes and slow by one node
    while (fast != NULL) {
        fast = fast->next;
        if (fast != NULL) {
            slow = slow->next;
            fast = fast->next;
        }
    }

    return slow;
}
```

## Data structures

### Binary search trees

Binary tree where all nodes in the left subtree are less than the node's value, and all nodes in the right subtree are greater than the node's value.

- Search, insertion deletion,
    - Average case: O(log(n))
    - Worst case: O(n) when tree is unbalanced, and essentially becomes a linked list

```Definition
typedef struct node {
    int data;
    struct node* left;
    struct node* right;
} node_t;
```

```Creation
node_t* createNode(int value) {
    node_t* newNode = (node_t*)malloc(sizeof(node_t));
    newNode->data = value;
    newNode->left = newNode->right = NULL;
    return newNode;
}
```

```Insertion
node_t* insert(node_t* root, int value) {
    if (root == NULL) {
        return createNode(value);
    }
    if (value < root->data) {
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        root->right = insert(root->right, value);
    }
    return root;
}
```

### Heaps

Max heap: the value of the parent node is >= to the value of its child nodes. Root node is the maximum value.

Min heap: the value of the parent node is <= to the value of its child nodes. Root node is the minimum value.

Index for array implementation of heap:
- Left child is at index `2 * i + 1`
- Right child is at index `2 * i + 2`
- Parent node is at index `(i - 1) / 2`

Advantage over linked lists: insertion and deletion can be done in O(log(n)) < O(n).

Insertion: O(log(n))
```
// Append new node to the end of the heap. Then:
void heapifyUp(int heap[], int index) {
    int parent = (index - 1) / 2;
    if (index > 0 && heap[index] > heap[parent]) {
        // Swap the current node with the parent
        int temp = heap[index];
        heap[index] = heap[parent];
        heap[parent] = temp;
        heapifyUp(heap, parent); // Recursively heapify the parent
    }
}
```

Deletion: O(log(n))
```
void heapifyDown(int heap[], int size, int index) {
    int largest = index;
    int left = 2 * index + 1;
    int right = 2 * index + 2;

    if (left < size && heap[left] > heap[largest]) {
        largest = left;
    }
    if (right < size && heap[right] > heap[largest]) {
        largest = right;
    }
    if (largest != index) {
        int temp = heap[index];
        heap[index] = heap[largest];
        heap[largest] = temp;
        heapifyDown(heap, size, largest); // Recursively heapify the affected subtree
    }
}
```

Heapify: O(n)
```
void buildHeap(int heap[], int size) {
    for (int i = (size / 2) - 1; i >= 0; i--) {
        heapifyDown(heap, size, i);
    }
}
```

### Priority queues

Implementation via a heap

### Linked lists

```Singly linked list
typedef struct Node {
    int data; 
    struct Node* next;
} node_t;
```

```Doubly linked list
typdef struct Node {
    int data;
    struct Node* next;
    struct Node* prev;
} node_t;
```

### Dictionaries

Dictionaries are often implmented using arrays, where the index is determined by a hash function applied on the key, providing efficient searching. Good hashing algorithms distribute the keys evenly throughout the array. However, collisions can occur.

Separate chaining to handle hash collisions. Each array index points to a linked list or binary tree that stores all key-value pairs that hash to the same index. Hence, in the absolute worst case, searching a dictionary using linked lists will be O(n) and those with binary trees are O(log(n)).

## Miscellaneous

Loop invariants: "In computer science, a loop invariant is a property of a program loop that is true before (and after) each iteration. It is a logical assertion, sometimes checked with a code assertion. Knowing its invariant(s) is essential in understanding the effect of a loop." (<https://en.wikipedia.org/wiki/Loop_invariant>)

Binary tree traversal methods
- In-order: Left-Node-Right
- Pre-order: Node-Left-Right
- Post-order: Left-Right-Node
