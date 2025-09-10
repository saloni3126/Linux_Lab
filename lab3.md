# ENHANCING AND CUSTOMIZING A SCRIPT
## objective:
to enhance the existing script file by allowing for start , end ,and step values,with input validation.
save the new version as 'file name.sh'
---

## ORIGINAL SCRIPT ('prime_number.sh'):
```bash
#!/bin/bash
# print_numbers.sh
# Prints numbers from 1 to 10

for i in {1..10}
do
    echo $i
done

```
---

## Behaviour
always print numbers from 1 to 10
no user input or flexibility.

![image](<Screenshot (13).png>)
---

## MODIFIED SCRIPT:
```BASH
#!/bin/bash
# enhanced_numbers.sh
# Prints numbers from a user-defined start to end using a positive step

# Prompt for input
read -p "Enter start number: " start
read -p "Enter end number: " end
read -p "Enter step value (must be positive): " step

# Validate inputs: all must be integers, step must be positive
if ! [[ "$start" =~ ^-?[0-9]+$ ]]; then
    echo "Error: Start must be an integer."
    exit 1
fi

if ! [[ "$end" =~ ^-?[0-9]+$ ]]; then
    echo "Error: End must be an integer."
    exit 1
fi

if ! [[ "$step" =~ ^[1-9][0-9]*$ ]]; then
    echo "Error: Step must be a positive integer."
    exit 1
fi

# Print numbers based on direction
if (( start <= end )); then
    for (( i=start; i<=end; i+=step )); do
        echo "$i"
    done
else
    for (( i=start; i>=end; i-=step )); do
        echo "$i"
    done
fi
```
![image](<Screenshot (14.png>)

## Enhanced Script Behavior

The enhanced script `enhanced_numbers.sh` prompts the user for three inputs:

- Start number (integer)
- End number (integer)
- Step (positive integer)

It validates the inputs for correct format and logical consistency (e.g., step must be positive). The script then prints numbers from start to end, increasing by the step value.

If the start is greater than the end, the script outputs a message and prints no numbers.



# features
accepts user input:start,end,and step.
validates the inputsare integers.
ensures step is positive number.
works for both ascending and decending ranges.

# example run :
* input 
```
enter start number:1
enter end number:10
enter step value :2
```
* output
```
1
3
5
9
```

# example (invalid)
* input
```
enter the start number:1
enter the end number:10
enter the step value:0
```
* output
```
error:step must be a positive integer.
```
---

| Aspect                      | Original Script (`print_numbers.sh`)                   | New Script (`enhanced_numbers.sh`)                                |
| --------------------------- | ------------------------------------------------------ | ----------------------------------------------------------------- |
| **Input**                   | No user input; hardcoded start=1, end=10, step=1       | User provides start, end, and step values at runtime              |
| **Range of numbers**        | Always prints numbers from 1 to 10                     | Prints numbers from user-defined start to end                     |
| **Step size**               | Fixed step size of 1                                   | Customizable positive integer step size                           |
| **Input validation**        | None                                                   | Validates that start and end are integers; step is positive       |
| **Behavior if start > end** | Not handled (prints nothing because of hardcoded loop) | Prints a message "Start is greater than end. No numbers printed." |
| **Flexibility**             | Very limited                                           | Flexible for different ranges and increments                      |

# Original Behavior

* The script printed numbers from 1 to 10 with a fixed step size of 1.

* No user input or validation.

* Behavior was static and limited.

# New Behavior

* The script prompts the user to input start, end, and step values.

* It validates that start and end are integers, and step is a positive integer.

* Prints numbers from start to end incremented by the step.

* If the start number is greater than the end number, it shows a message and prints no numbers.

* Provides much more flexibility and error handling.


## conclusion
* the new script is much more flexible and user friendly .it allows dynamic input,validates values,and can count up or down. this makes it as useful general-purpose number-printing tool

* The enhanced script `enhanced_numbers.sh` significantly improves upon the original by adding user interactivity and input validation. Allowing the user to specify start, end, and step values makes the script much more flexible and practical for various use cases. Additionally, validating inputs ensures the script runs reliably and prevents invalid operations, such as using a non-positive step. Overall, this enhancement transforms a simple fixed-range script into a versatile tool that adapts to user requirements.

---

## Extra questions
1. Difference between $1, $@, and $# in bash?
* $1 :

-Represents the first positional parameter (i.e., the first argument passed to the script or function).

-Example: If you run ./script.sh apple banana, then $1 is apple.

* $@:

-Represents all positional parameters passed to the script as separate quoted strings.

-When you use "$@" (with quotes), each argument is preserved as a separate word, even if they contain spaces.

-Example: If you run ./script.sh "apple pie" banana, then "$@" expands to "apple pie" and "banana" (two separate arguments).

* $# :

-Represents the number of positional parameters passed to the script.

-Example: If you run ./script.sh apple banana, then $# is 2.


---
| Variable | Meaning                           | Example (args: apple "banana split") |
| -------- | --------------------------------- | ------------------------------------ |
| `$1`     | First argument                    | apple                                |
| `$@`     | All arguments as separate strings | `"apple"` `"banana split"`           |
| `$#`     | Number of arguments               | 2                                    |
---

2. What does exit 1 mean in a script?


In a script, `exit 1` means:

* **Exit the script immediately**, stopping any further execution.
* Return an **exit status code of 1** to the shell or calling process.
* By convention, an exit code of `0` means **success**, and **any non-zero value (like 1)** indicates **an error or abnormal termination**.

So when you write:

```bash
if [ some_error_condition ]; then
  echo "Error: something went wrong"
  exit 1
fi
``` 
---
