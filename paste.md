## summary Script issue with skipping certain files in Bash <br>
---<br>
It looks like there are a few key points in your script that may be contributing to the failure to skip files as intended. Below, I'll break down the sections of your script for better understanding and troubleshooting.

1
---  
**Defining the Variables**  
```bash
JOHN_PKG_BUILD_SCRIPT=$(find "./packages/$JOHN_PKG_TO_BUILD" -type f -name "build.sh")
JOHN_PKG_DEPENDS=$(grep "TERMUX_PKG_DEPENDS" $JOHN_PKG_BUILD_SCRIPT | sed -n 's/.*"\([^"]*\)".*/\1/p')
```
- **`JOHN_PKG_BUILD_SCRIPT`**: This line finds the `build.sh` script within the specified package directory.
- **`JOHN_PKG_DEPENDS`**: This extracts dependencies listed in the `build.sh` script using `grep` to find the `TERMUX_PKG_DEPENDS` line and `sed` to clean up the output.

2
---  
**Handling Dependencies**  
```bash
IFS="," read -r -a dep_array <<< "$JOHN_PKG_DEPENDS"
exclude_these_script=("fontconfig")
```
- **`IFS=","`**: Sets the Internal Field Separator to a comma, allowing you to read a comma-separated string into an array.
- **`exclude_these_script`**: An array containing scripts that you want to exclude from processing.

3
---  
**Checking Dependency Against Excluded Scripts**  
```bash
for dep_ in "${dep_array[@]}"; do
  dep_=$(echo "$dep_" | xargs) #remove white space from beginning
  if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
    continue
  fi
```
- The line `dep_=$(echo "$dep_" | xargs)` trims whitespace from the beginning and end of the dependency.
- The conditional check `if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]` attempts to check if the current dependency is in the `exclude_these_script` array. However, this matches a string pattern in a way that may not work as expected—this is likely the source of the problem.

4
---  
**Fixing the Exclusion Logic**  
To correctly check if a dependency is excluded, you can use a nested loop. The corrected code is as follows:
```bash
skip_dependency=false
for dep_ in "${dep_array[@]}"; do
  dep_=$(echo "$dep_" | xargs)
  for excluded in "${exclude_these_script[@]}"; do
    if [[ "$dep_" == "$excluded" ]]; then
      skip_dependency=true
      break 2 # Break out of both loops
    fi
  done
done
```
- This nested loop checks each dependency against the excluded scripts more reliably.  
- Using `break 2` will exit both loops if a match is found.

5
---  
**Adding Scripts**  
```bash
for p in "${build_script_paths[@]}"; do
  echo "$add_script" >> "$p"
done
```
- This loops through the collected scripts and appends the `add_script` content to each of them. 

### Example
You can test the script by modifying the `exclude_these_script` array and running it on a package with various dependencies. Make sure that `fontconfig` is indeed a dependency of a package you are testing.

### Conclusion
Make sure to implement the suggested changes in how you exclude the dependencies. After these adjustments, your script should work as intended without incorrectly processing the excluded files. 

## references:
## http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_02_04.html  
## http://tldp.org/LDP/abs/html/internalfieldseparators.html  