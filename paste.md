## Summary
The error message indicates a syntax issue in a shell script (`pre_conf_build.sh`) on line 12, specifically within a conditional expression.

---

1
The error message states: 
```
syntax error in conditional expression: unexpected token `;'
```
This suggests that there is a problem with the syntax of the `if` statement on line 12. The specific line in question is:
```bash
if [[ "${exclude_these_script[@]}" =~ " $dep_ "]]; then
```

---

2
The issue arises from the placement of the closing brackets `]]`. In Bash, when using the `[[ ... ]]` construct for conditional expressions, there should be a space before the closing brackets. The correct syntax should look like this:
```bash
if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
```
Notice the space before the `]]`.

---

3
Additionally, the use of the `=~` operator is for regex matching in Bash. Ensure that the variable `exclude_these_script` is an array and that the regex pattern is correctly formatted. If you want to check if any element in the array matches the pattern, you might need to iterate through the array or adjust the regex accordingly.

---

Example:
Here’s a corrected version of the line:
```bash
if [[ "${exclude_these_script[@]}" =~ " $dep_ " ]]; then
```
Make sure to test the script after making this change to confirm that the error is resolved.

---

References:
## https://www.gnu.org/software/bash/manual/bash.html#Conditional-Expressions
## https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_05.html