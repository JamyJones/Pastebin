## Summary
The error message indicates a syntax issue in a shell script, specifically in a conditional expression on line 12 of the `pre_conf_build.sh` script.

---
Explanation:

1. **Understanding the Error Message**  
   The error message states that there is a "syntax error in conditional expression" and points to an unexpected token `;`. This typically means that the shell is unable to parse the conditional statement correctly due to improper syntax.

2. **Analyzing the Conditional Statement**  
   The problematic line is:
   ```bash
   if [[ "${exclude_these_script[@]}" =~ " $dep_ "]]; then
   ```
   Here are the issues:
   - **Spacing**: There should be a space before the closing brackets `]]`. The correct syntax should be:
     ```bash
     if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
     ```
   - **Quoting**: The use of quotes around the variable `$dep_` may not be necessary depending on the context. If `$dep_` is meant to be a variable, ensure it is defined properly.

3. **Correcting the Syntax**  
   To fix the error, modify the line as follows:
   ```bash
   if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
   ```
   Ensure that:
   - There is a space before `]]`.
   - The variable `$dep_` is defined and holds the expected value.

4. **General Tips for Debugging Shell Scripts**  
   - **Use `set -x`**: This command can be added at the beginning of your script to enable debugging, which will print each command before it is executed.
   - **Check for Unmatched Quotes or Brackets**: Ensure that all quotes and brackets are properly matched.
   - **Run ShellCheck**: This is a useful tool for analyzing shell scripts for potential issues.

---
Example:
Here’s a corrected version of the conditional statement in context:
```bash
exclude_these_script=("script1" "script2")
dep_="script1"

if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
    echo "$dep_ is excluded."
else
    echo "$dep_ is not excluded."
fi
```
In this example, if `dep_` matches any of the scripts in `exclude_these_script`, it will print that it is excluded.

---
References:
## https://www.shellcheck.net/  
## https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_05.html