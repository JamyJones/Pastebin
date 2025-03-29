## Accessing Elements in an Array in Bash
---  
In Bash, arrays are a powerful way to store multiple values in a single variable. Accessing elements in an array is straightforward, and you can easily retrieve the first element, last element, or any specific index.

1  
---  
**Declaring an Array**  
To declare an array in Bash, you can use the following syntax:  
```bash
my_array=(value1 value2 value3)
```
- `my_array` is the name of the array.
- The parentheses `()` are used to enclose the values.
- Each value is separated by a space.

2  
---  
**Accessing Elements**  
To access elements in the array, you use the syntax `${array_name[index]}`. Here’s how you can access specific elements:  
- **First Element**:  
```bash
echo ${my_array[0]}
```
  - This retrieves the first element of the array. In Bash, array indexing starts at 0.

- **Second Element**:  
```bash
echo ${my_array[1]}
```
  - This retrieves the second element of the array.

- **Last Element**:  
To access the last element, you can use the following method:  
```bash
echo ${my_array[-1]}
```
  - The `-1` index refers to the last element in the array.

3  
---  
**Example of Accessing Array Elements**  
Here’s a complete example that demonstrates how to declare an array and access its elements:  
```bash
#!/bin/bash

# Declare an array
fruits=("apple" "banana" "cherry" "date")

# Accessing elements
echo "First fruit: ${fruits[0]}"  # Outputs: apple
echo "Second fruit: ${fruits[1]}" # Outputs: banana
echo "Last fruit: ${fruits[-1]}"   # Outputs: date
```
- In this script, we declare an array called `fruits` with four elements.
- We then use `echo` to print the first, second, and last elements of the array.

4  
---  
**Iterating Over an Array**  
You can also iterate over all elements in an array using a loop:  
```bash
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done
```
- Here, `${fruits[@]}` expands to all elements in the array.
- The loop prints each fruit on a new line.

## References  
## https://www.gnu.org/software/bash/manual/bash.html#Arrays  
## https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_05.html  