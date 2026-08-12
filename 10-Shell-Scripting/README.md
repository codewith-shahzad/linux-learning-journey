# Shell Scripting (Part 1)

## What this is
A shell script is a text file containing a sequence of commands, saved together so they can all run at once instead of being typed one by one. This is the foundation of automation — the same idea that later scales up to CI/CD pipelines.

## Core building blocks

### The shebang line
```bash
#!/bin/bash
```
Always the first line of a script. Tells Linux which interpreter to run the script with. Note: `#!/bin/bin` (missing `/bash`) causes a "cannot execute: required file not found" error — an easy typo to make, worth double-checking.

### Making a script executable
```bash
chmod +x myscript.sh
./myscript.sh
```
Ties back to the Permissions module — without execute permission, the file is just readable text, not something Linux will run as a program.

### Variables
```bash
name="Shahzad"
echo "Hello, $name"
```
- No spaces around `=` when setting a variable
- Use `$name` to read its value; without the `$`, Bash treats it as literal text

### Reading user input
```bash
echo "What is your name?"
read username
echo "Nice to meet you, $username"
```
`read` pauses the script and stores whatever the user types into the named variable.

### Conditionals
```bash
if [ $age -ge 18 ]
then
    echo "You are an adult"
else
    echo "You are a minor"
fi
```
Comparison operators for numbers: `-ge` (≥), `-le` (≤), `-gt` (>), `-lt` (<), `-eq` (=). Spaces are required right after `[` and before `]`.

### Loops
**`for`** — repeats over a fixed list, stops automatically once the list is exhausted:
```bash
for i in 1 2 3 4 5
do
    echo "Number: $i"
done
```

**`while`** — repeats as long as a condition stays true; needs a line inside it that eventually makes the condition false, or it becomes an infinite loop:
```bash
count=1
while [ $count -le 5 ]
do
    echo "Count: $count"
    count=$((count + 1))
done
```
`$(( ))` is Bash's arithmetic syntax — without it, `count + 1` is treated as plain text, not math.

### Functions
```bash
greet() {
    echo "Hello, $1"
}

greet "Shahzad"
greet "Ali"
```
- `$1`, `$2`, etc. refer to the arguments passed in when the function is called
- A function must be **defined before it's called**, and it must be called from **inside the same script** — it only exists while that script is running, and isn't available in the terminal afterward

## What I tested
1. Wrote and ran a first script using shebang, `echo`, and `date`.
2. Added a variable and printed it with `$name`.
3. Added `read` to take user input interactively.
4. Built an if/else conditional checking age — debugged a shebang typo (`#!/bin/bin`) independently using `cat -A` to inspect the file.
5. Wrote and ran both a `for` loop and a `while` loop, correctly reasoning that `for` loops self-limit (fixed list) while `while` loops can run infinitely if the condition is never updated.
6. Wrote a `greet()` function using `$1` — initially tried calling it from the terminal after the script finished (didn't work, since functions don't persist after the script ends), then corrected by calling it from inside the script.

## Note-to-self
- "Cannot execute: required file not found" almost always means a bad shebang line, not a missing/misnamed script file.
- Functions and variables set inside a script don't persist in the terminal after the script finishes running.

## Still to cover (Part 2)
Arrays, case statements, exit codes (`$?`), and a combined practice script using a function + loop + conditional together.
