A variable is an identifier associated with a memory location.
 It allows programmers to store, retrieve, and manipulate data without needing to know the exact memory address.
A normal variable is a named storage location in memory that directly contains a value.for example
                int x=5;
where x is a variable of type int that stores the value 5.
The compiler associates x with a specific memory address, and operations on x directly manipulate the value stored at that address.
A pointer is a variable that stores a memory address rather than a direct value.
A variable is a container of data; a pointer is a reference to a container.
for example:
                                int k; → Declares a variable k of type integer. At this point, memory is reserved for k, but its value is undefined until assigned.
                                k = 5; → Defines the value of k. Now the memory location associated with k contains the integer 5.
                                int* pk; → Declares a pointer named pk that can store the address of an integer variable
At this stage, pk exists but does not yet point to anything meaningful (it is uninitialized).
&k → The address-of operator retrieves the memory address of k.
                                pk = &k; → Assigns that address to the pointer pk. Now pk holds the location of k.
A pointer, then, is a data item whose value is a memory address and type describes the data located at that memory address. As such, pointers allow data structures and/or variables to be
shared among functions.  
The simplest pointer available to us in C is the NULL pointer.
When you create a pointer and you don’t set its value immediately, you should always set the value of the pointer to NULL.
You can check whether a pointer is NULL using the equality operator (==).
Another easy way to create a pointer is to simply extract the address of an already existing variable. We can do this with the address extraction operator (&).
If x is an int-type variable, then &x is a pointer-to-int whose value is the address of x.
If arr is an array of doubles, then &arr[i] is a pointer-to- double who value is the address of the ith element of arr
The main purpose of a pointer is to allow us to modify or inspect the location to which it points. We do this by dereferencing the pointer.
  deferencing a pointer means accessing the value stored at the memory address contained in the pointer.
If we have a pointer-to-char called pc, then *pc is the data that lives at the memory address stored inside the variable pc.
Used in this context, * is known as the dereference operator.
It “goes to the reference” and accesses the data at that memory location, allowing you to manipulate it at will.
for example:
              int k = 5;       // Declare and define an integer variable
              int *pk = &k;    // pk stores the address of k
              printf("%d\n", *pk);  // Output: 5 (dereferencing pk)
              *pk = 10;             // Modify value at address pk points to
              printf("%d\n", k);    // Output: 10 (k is now changed)
so here pk holds the address of k.
       *pk accesses the value at that address.
       *pk = 10; changes the value of k indirectly. 
Pointers are preferred over normal variables when indirect memory access, dynamic data manipulation, or efficient parameter passing is required.
for instances, You want a function to modify the original variables passed to it.
code
                                 #include <stdio.h>
                                  void swap(int *a, int *b) {
                                    int temp = *a;
                                    *a = *b;
                                    *b = temp;
                                   }

                                int main() {
                                int x = 10, y = 20;
                                swap(&x, &y);
                                printf("x = %d, y = %d\n", x, y);  // Output: x = 20, y = 10
                                return 0;
                                }
Pointers allow direct access to the caller’s memory, enabling in-place modification.Whereas, Normal variables are passed by value, meaning changes inside the function don’t affect the original. 
                                
                                 limitation and risk associated with using pointers compared to variables.
Pointers require careful handling of addresses and dereferencing.
Code becomes harder to read, debug, and maintain compared to simple variables.  
Dynamically allocated memory (malloc, calloc) must be freed manually.
Forgetting free() causes memory leaks, wasting system resources.
Dereferencing a NULL pointer causes segmentation faults or runtime errors.
Normal variables cannot be “null”; they always map to valid memory.
Heavy pointer use reduces readability and increases the chance of subtle bugs.
variables are simpler and safer for direct data storage.    

          call in value
when a function is called, the actual value of the argument is copied into the function’s parameter.
The function works with this copy, so changes made inside the function do not affect the original variable.
for example:
        #include <stdio.h>
        void modify(int x) {
        x = x + 10;   // modifies only the local copy
        }

        int main() {
          int a = 5;
          modify(a);
          printf("a = %d\n", a);  // Output: a = 5
          return 0;
        }
Explanation:
             a is passed to modify().
             Inside the function, x is a copy of a.
            Changing x does not change a in main.
output-main:   a = 5
               modify: x = 5 → changed to 15, but discarded after function ends            

                       call by reference
 In call by reference, instead of passing a copy of the value, the function receives the address of the variable.
 This allows the function to directly access and modify the original variable.                      
    for example:       
                       #include <stdio.h>
                       void modify(int *x) {
                       *x = *x + 10;   // modifies the original variable via pointer
                        }

                       int main() {
                            int a = 5;
                            modify(&a);
                            printf("a = %d\n", a);  // Output: a = 15
                            return 0;
                       }
Explanation:
           &a passes the address of a to modify().
           Inside the function, x is a pointer to a.
           *x dereferences the pointer, directly modifying a.
output:
        main:   a = 5 → modified to 15
        modify: x = address of a → *x accesses and changes a

Here are the practical scenarios where Call by value is preferred and Call by reference is preferred.
                            Call by reference is preferred
it is prefered when:
                       the function must update the caller’s variable.
                       Passing large structures or arrays by reference avoids costly copying.
 for example:
                   #include <stdio.h>
                   void swap(int *a, int *b) {
                   int temp = *a;
                   *a = *b;
                   *b = temp;
                   }

                  int main() {
                      int x = 10, y = 20;
                      swap(&x, &y);
                      printf("x = %d, y = %d\n", x, y);
                      // Output: x = 20, y = 10
                  }

                  call by value
it is prefered when:
                    you want to ensure the original variable remains unchanged.
                    the function only needs to work with temporary data.
for example:
                     #include <stdio.h>
                    int square(int x) {
                    return x * x;   // works on a copy
                    }

                    int main() {
                        int num = 5;
                        int result = square(num);
                        printf("num = %d, result = %d\n", num, result);
                        // Output: num = 5, result = 25
                    }
num remains unchanged because the function only receives a copy.
This is safer when you don’t want the function to accidentally alter the original data.
