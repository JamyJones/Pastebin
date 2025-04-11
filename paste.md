## Hello Program using `getgrgid_r()` in C <br>
---<br>
The `getgrgid_r()` function is used to get information about a group based on its group ID (gid). It is defined in the `<grp.h>` header file and provides a thread-safe way to retrieve group information. The primary purpose of this function is to fill a `group` structure with the details of a group corresponding to a specified group ID.

**Note:** You need to include `<stdlib.h>` for memory allocation and error checking, and `<stdio.h>` for standard input/output.

Here’s a simple program that demonstrates how to use `getgrgid_r()` to retrieve and print information about a group.

```c
#include <stdio.h>
#include <stdlib.h>
#include <grp.h>
#include <unistd.h>
#include <string.h>

int main() {
    gid_t gid = getgid(); // Get the current group ID
    struct group grp; // Create a group structure
    struct group *result; // Pointer to hold the result
    char buf[1024]; // Buffer to store the returned data
    int errnum; // Variable to hold error numbers

    // Get the group information using getgrgid_r
    errnum = getgrgid_r(gid, &grp, buf, sizeof(buf), &result);
    
    // Check for errors
    if (errnum != 0) {
        perror("getgrgid_r error");
        return 1;
    }

    // Check if the group was found
    if (result == NULL) {
        printf("Group ID %d not found.\n", gid);
        return 1;
    }

    // Print group information
    printf("Group Name: %s\n", grp.gr_name);
    printf("Group ID: %d\n", grp.gr_gid);
    printf("Group Members: ");
    
    // Print the members of the group
    char **members = grp.gr_mem;
    if (*members) {
        for (int i = 0; members[i] != NULL; i++) {
            printf("%s ", members[i]);
        }
    } else {
        printf("(No members)");
    }
    printf("\n");

    return 0;
}
```

---<br>
Explanation:
1
---  
`#include <stdio.h>`: This line includes the standard I/O library for input and output functions, such as `printf` and `perror`.

2
---  
`#include <stdlib.h>`: This line includes the standard library, which contains functions for memory allocation and other utilities.

3
---  
`#include <grp.h>`: This header file defines the structure and function prototypes for group-related calls, including `getgrgid_r()`.

4
---  
`#include <unistd.h>`: This header file provides access to the POSIX operating system API, which includes the `getgid()` function to get the current process's group ID.

5
---  
`#include <string.h>`: This header file provides functions for handling strings, though it's not strictly necessary for this example.

6
---  
`gid_t gid = getgid();`: This line gets the current user’s group ID and stores it in `gid`.

7
---  
`struct group grp;`: This line declares a structure `grp` of type `struct group`, which will hold the group information.

8
---  
`struct group *result;`: This line declares a pointer `result` to hold the return value of `getgrgid_r()`.

9
---  
`char buf[1024];`: This creates a buffer for storing the group information returned by `getgrgid_r()`.

10
---  
`int errnum;`: This variable is used to capture any potential errors that occur during the call.

11
---  
`errnum = getgrgid_r(gid, &grp, buf, sizeof(buf), &result);`: This line calls `getgrgid_r()` with the current group ID to fill the `grp` structure with the group details into the buffer `buf`.

12
---  
`if (errnum != 0)`: This checks if the `getgrgid_r()` function returned an error.

13
---  
`if (result == NULL)`: This checks whether the group was found.

14
---  
`printf("Group Name: %s\n", grp.gr_name);`: This prints the name of the group.

15
---  
`printf("Group ID: %d\n", grp.gr_gid);`: This prints the group's ID.

16
---  
The code in the loop prints the members of the group. If there are no members, it outputs "(No members)".

This program, when compiled and executed, will show the group information of the current process based on the group ID. You can use a C compiler such as `gcc` to compile the code.

---  
References: 
## https://man7.org/linux/man-pages/man3/getgrent.3.html 
## https://man7.org/linux/man-pages/man3/getgrgid.3.html 
## https://man7.org/linux/man-pages/man3/getgrgid_r.3.html