## Hello Program Using `getgrgid_r()` in C
<br>
This program demonstrates how to use the `getgrgid_r()` function in C to retrieve group information based on a group ID. The `getgrgid_r()` function is part of the POSIX standard and is used to get the group information in a thread-safe manner.

### Explanation:
1
---
**Include Necessary Headers**  
To use `getgrgid_r()`, you need to include the following headers:
```c
#include <stdio.h>      // For printf
#include <stdlib.h>     // For malloc and free
#include <string.h>     // For memset
#include <grp.h>        // For getgrgid_r and struct group
#include <unistd.h>     // For getgid
```
- `stdio.h`: Provides functionalities for input and output.
- `stdlib.h`: Contains functions for memory allocation.
- `string.h`: Used for string manipulation functions like `memset`.
- `grp.h`: Contains the definition of the `getgrgid_r()` function and the `group` structure.
- `unistd.h`: Provides access to the POSIX operating system API, including `getgid()`.

2
---
**Define the Main Function**  
The main function is where the program execution begins.
```c
int main() {
    gid_t gid = getgid(); // Get the current group ID
    struct group grp;     // Declare a group structure
    struct group *result; // Pointer to hold the result
    char buffer[1024];    // Buffer for the group name
    int s;                // Status variable
```
- `getgid()`: Retrieves the group ID of the calling process.
- `struct group`: A structure that holds information about a group.
- `char buffer[1024]`: A buffer to store the group name.
- `int s`: A variable to store the return status of `getgrgid_r()`.

3
---
**Retrieve Group Information**  
Use `getgrgid_r()` to get the group information based on the group ID.
```c
    s = getgrgid_r(gid, &grp, buffer, sizeof(buffer), &result);
    if (s != 0) {
        perror("getgrgid_r failed"); // Print error if it fails
        return 1; // Exit with error code
    }
```
- `getgrgid_r(gid, &grp, buffer, sizeof(buffer), &result)`: This function retrieves the group information for the specified group ID (`gid`). It fills the `grp` structure with the group details, uses `buffer` to store the group name, and `result` to point to the filled structure.
- `perror()`: Prints a description for the last error that occurred.

4
---
**Display the Group Name**  
Finally, print the group name to the console.
```c
    printf("Group ID: %d\n", gid);
    printf("Group Name: %s\n", grp.gr_name); // Print the group name
    return 0; // Exit successfully
}
```
- `printf()`: Used to output the group ID and group name to the console.

### Complete Code Example:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <grp.h>
#include <unistd.h>

int main() {
    gid_t gid = getgid(); // Get the current group ID
    struct group grp;     // Declare a group structure
    struct group *result; // Pointer to hold the result
    char buffer[1024];    // Buffer for the group name
    int s;                // Status variable

    s = getgrgid_r(gid, &grp, buffer, sizeof(buffer), &result);
    if (s != 0) {
        perror("getgrgid_r failed"); // Print error if it fails
        return 1; // Exit with error code
    }

    printf("Group ID: %d\n", gid);
    printf("Group Name: %s\n", grp.gr_name); // Print the group name
    return 0; // Exit successfully
}
```
### How to Compile and Run:
To compile the program, use the following command in your terminal:
```bash
gcc -o hello_group hello_group.c
```
Then, run the program:
```bash
./hello_group
```

### References:
## https://man7.org/linux/man-pages/man3/getgrgid.3.html  
## https://man7.org/linux/man-pages/man3/getgrgid_r.3.html  