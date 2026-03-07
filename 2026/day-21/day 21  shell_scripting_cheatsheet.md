
Task 1: Basics
Document the following with short descriptions and examples:

Shebang (#!/bin/bash) — what it does and why it matters
1. Shebang (#!/bin/bash)
Description

A shebang is the first line in a script that tells the operating system which interpreter should run the script.

When the script is executed directly, the system reads this line and uses the specified interpreter.

Running a script — chmod +x, ./script.sh, bash script.sh
Method 2: Run with Bash interpreter
bash script.sh

This does not require executable permission.

Example Script
#!/bin/bash
echo "This script is running!"

Run:

chmod +x script.sh
./script.sh

Comments — single line (#) and inline
Comments

Comments are used to explain code and are ignored by the shell.

Single-line Comment

Use #.

# This is a comment
echo "Hello"
Inline Comment
echo "Hello"  # This prints Hello

Comments help improve readability and maintainability.

Variables — declaring, using, and quoting ($VAR, "$VAR", '$VAR')
4. Variables

Variables store values that can be reused in the script.

Declaring Variables
NAME="Rohit"
AGE=25

⚠️ No spaces around =.

Using Variables

Use $ to access the value.

echo $NAME
echo $AGE

Example:

NAME="Rohit"
echo "Hello $NAME"

Output:

Hello Rohit
Quoting Variables
Double Quotes " "

Allows variable expansion.

NAME="Rohit"
echo "Hello $NAME"

Output:

Hello Rohit
Single Quotes ' '

Prevents variable expansion.

NAME="Rohit"
echo 'Hello $NAME'

Output:

Hello $NAME
Best Practice

Use:

"$VAR"

This prevents issues with spaces.

Example:

FILE="my file.txt"
cat "$FILE"



Reading user input — read
5. Reading User Input (read)

The read command allows a script to accept user input.

Example
#!/bin/bash

echo "Enter your name:"
read NAME

echo "Hello $NAME"

Run:

Enter your name:
Rohit
Hello Rohit
Example with Prompt
read -p "Enter your age: " AGE
echo "You are $AGE years old."


Command-line arguments — $0, $1, $#, $@, $?
6. Command-Line Arguments

Arguments are values passed to a script when running it.

Example:

./script.sh Rohit 25
$0

Name of the script.

Example:

echo "Script name: $0"
$1, $2, $3

First, second, third arguments.

Example:

#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"

Run:

./script.sh apple banana

Output:

First argument: apple
Second argument: banana
$#

Number of arguments passed.

Example:

echo "Total arguments: $#"
$@

Represents all arguments.

Example:

echo "All arguments: $@"
$?

Shows the exit status of the last command.

Example:

ls file.txt
echo $?

Output:

0   # success
1   # error
Summary
Symbol	Meaning
#!/bin/bash	Specifies Bash interpreter
chmod +x	Makes script executable
#	Comment
$VAR	Access variable
read	Take user input
$0	Script name
$1, $2	Arguments
$#	Number of arguments
$@	All arguments
$?	Last command exit status



Task 2: Operators and Conditionals
Document with examples:

String comparisons — =, !=, -z, -n
1. String Comparisons

String comparison operators are used to compare text values inside conditional statements.

Operator	Meaning
=	Equal
!=	Not equal
-z	String is empty
-n	String is not empty
Example
#!/bin/bash

name="Rohit"

if [ "$name" = "Rohit" ]; then
    echo "Names match"
fi

Output

Names match


Integer comparisons — -eq, -ne, -lt, -gt, -le, -ge
2. Integer Comparisons

Used to compare numbers in Bash.

Operator	Meaning
-eq	Equal
-ne	Not equal
-lt	Less than
-gt	Greater than
-le	Less than or equal
-ge	Greater than or equal
Example
#!/bin/bash

num=10

if [ $num -gt 5 ]; then
    echo "Number is greater than 5"
fi
Example (Equality)
#!/bin/bash

a=10
b=10

if [ $a -eq $b ]; then
    echo "Numbers are equal"
fi

File test operators — -f, -d, -e, -r, -w, -x, -s
3. File Test Operators

These operators check file properties.

Operator	Meaning
-f	File exists and is regular file
-d	Directory exists
-e	File exists
-r	File is readable
-w	File is writable
-x	File is executable
-s	File is not empty
Example (Check File)
#!/bin/bash

file="test.txt"

if [ -f "$file" ]; then
    echo "File exists"
else
    echo "File not found"
fi
Example (Check Directory)
#!/bin/bash

dir="Documents"

if [ -d "$dir" ]; then
    echo "Directory exists"
fi
Example (Check File Size)
#!/bin/bash

file="data.txt"

if [ -s "$file" ]; then
    echo "File is not empty"
fi


if, elif, else syntax
4. if, elif, else Syntax

Conditional statements allow scripts to make decisions.

Syntax
if [ condition ]
then
    command
elif [ condition ]
then
    command
else
    command
fi
Example
#!/bin/bash

num=15

if [ $num -lt 10 ]; then
    echo "Number is less than 10"
elif [ $num -eq 10 ]; then
    echo "Number is equal to 10"
else
    echo "Number is greater than 10"
fi


Logical operators — &&, ||, !
5. Logical Operators

Logical operators combine multiple conditions.

Operator	Meaning
&&	Logical AND
`	
!	Logical NOT
AND Example
#!/bin/bash

age=25
country="India"

if [ $age -gt 18 ] && [ "$country" = "India" ]; then
    echo "Eligible"
fi
OR Example
#!/bin/bash

num=5

if [ $num -lt 10 ] || [ $num -eq 20 ]; then
    echo "Condition satisfied"
fi
NOT Example
#!/bin/bash

num=10

if [ ! $num -eq 5 ]; then
    echo "Number is not 5"
fi


Case statements — case ... esac
6. Case Statements

The case statement is used for multiple conditions and is often cleaner than many if statements.

Syntax
case variable in
    pattern1)
        command
        ;;
    pattern2)
        command
        ;;
    *)
        default command
        ;;
esac
Example
#!/bin/bash

read -p "Enter a number (1-3): " num

case $num in
    1)
        echo "You selected One"
        ;;
    2)
        echo "You selected Two"
        ;;
    3)
        echo "You selected Three"
        ;;
    *)
        echo "Invalid option"
        ;;
esac
Summary
Category	Operators
String	=, !=, -z, -n
Integer	-eq, -ne, -lt, -gt, -le, -ge
File Tests	-f, -d, -e, -r, -w, -x, -s
Logical	&&, `
Conditional	if, elif, else
Multi-condition	case ... esac



Task 3: Loops
Document with examples:

for loop — list-based and C-style
1. for Loop

A for loop is used when you want to iterate over a list of values.

List-Based for Loop
Syntax
for variable in list
do
    commands
done
Example
#!/bin/bash

for fruit in apple banana mango
do
    echo "Fruit: $fruit"
done
Output
Fruit: apple
Fruit: banana
Fruit: mango
C-Style for Loop

Similar to loops in C or Java.

Syntax
for (( initialization; condition; increment ))
do
    commands
done
Example
#!/bin/bash

for ((i=1; i<=5; i++))
do
    echo "Number: $i"
done
Output
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5

while loop
2. while Loop

A while loop runs as long as a condition remains true.

Syntax
while [ condition ]
do
    commands
done
Example
#!/bin/bash

count=1

while [ $count -le 5 ]
do
    echo "Count: $count"
    ((count++))
done
Output
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5

until loop
3. until Loop

An until loop runs until the condition becomes true (opposite of while).

Syntax
until [ condition ]
do
    commands
done
Example
#!/bin/bash

num=1

until [ $num -gt 5 ]
do
    echo "Number: $num"
    ((num++))
done
Output
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5



Loop control — break, continue
4. Loop Control Statements
break

Stops the loop immediately.

Example
#!/bin/bash

for i in 1 2 3 4 5
do
    if [ $i -eq 3 ]
    then
        break
    fi
    echo $i
done
Output
1
2
continue

Skips the current iteration and moves to the next one.

Example
#!/bin/bash

for i in 1 2 3 4 5
do
    if [ $i -eq 3 ]
    then
        continue
    fi
    echo $i
done
Output
1
2




Looping over files — for file in *.log
5. Looping Over Files

You can loop through files in a directory using wildcards.

Example
#!/bin/bash

for file in *.log
do
    echo "Processing $file"
done

This loop processes all .log files in the current directory.

Example output:

Processing system.log
Processing error.log
Processing access.log


Looping over command output — while read line
6. Looping Over Command Output

You can read output line-by-line using while read.

Example
#!/bin/bash

cat file.txt | while read line
do
    echo "Line: $line"
done

If file.txt contains:

Apple
Banana
Mango

Output:

Line: Apple
Line: Banana
Line: Mango
Better Practice Version
while read line
do
    echo "$line"
done < file.txt

This avoids unnecessary use of cat.

Summary
Loop Type	Purpose
for	Iterate over a list or range
for (( ))	C-style numeric loop
while	Run while condition is true
until	Run until condition becomes true
break	Exit loop early
continue	Skip current iteration
for file in *.log	Loop over files
while read	Process command/file output line by line

Task 4: Functions
Document with examples:

Defining a function — function_name() { ... }
1. Defining a Function

A function is defined using the syntax:

Syntax
function_name() {
    commands
}
Example
#!/bin/bash

greet() {
    echo "Hello, welcome to Bash scripting!"
}

This function named greet prints a greeting message.

Calling a function
2. Calling a Function

To execute a function, simply write its name.

Example
#!/bin/bash

greet() {
    echo "Hello, welcome to Bash scripting!"
}

greet
Output
Hello, welcome to Bash scripting!

The function runs when the script reaches the line greet.


Passing arguments to functions — $1, $2 inside functions
3. Passing Arguments to Functions

Functions can receive arguments similar to scripts. Inside the function, arguments are accessed using $1, $2, $3, etc.

Example
#!/bin/bash

greet() {
    echo "Hello $1"
}

greet Rohit
Output
Hello Rohit

Example with multiple arguments:

#!/bin/bash

add_numbers() {
    result=$(($1 + $2))
    echo "Sum: $result"
}

add_numbers 5 10
Output
Sum: 15

Return values — return vs echo
4. Return Values — return vs echo

Bash functions can provide output in two ways.

Using return

return sends an exit status (0–255) back to the caller.

Example
#!/bin/bash

check_number() {
    if [ $1 -gt 10 ]; then
        return 0
    else
        return 1
    fi
}

check_number 15
echo $?
Output
0

Here:

0 means success

Non-zero means failure

Using echo

echo is commonly used to return actual values or data.

Example
#!/bin/bash

add() {
    sum=$(($1 + $2))
    echo $sum
}

result=$(add 4 6)
echo "Result: $result"
Output
Result: 10


Local variables — local
5. Local Variables

Variables inside functions are global by default in Bash. To limit them to a function, use the local keyword.

Syntax
local variable_name
Example
#!/bin/bash

my_function() {
    local message="This is a local variable"
    echo $message
}

my_function

The variable message exists only inside the function.

Example Showing Local vs Global
#!/bin/bash

name="Global"

test_function() {
    local name="Local"
    echo "Inside function: $name"
}

test_function
echo "Outside function: $name"
Output
Inside function: Local
Outside function: Global

This shows that local prevents variables from affecting the rest of the script.

Summary
Concept	Description
function_name() {}	Defines a function
function_name	Calls the function
$1, $2	Access function arguments
return	Returns exit status (0–255)
echo	Returns actual output/data
local	Creates variables limited to the function


Task 5: Text Processing Commands
Document the most useful flags/patterns for each:

grep — search patterns, -i, -r, -c, -n, -v, -E
1. grep — Search for Patterns

grep searches for text patterns inside files.

Basic Syntax
grep pattern file
Useful Flags
Flag	Description
-i	Case-insensitive search
-r	Recursive search in directories
-c	Count matching lines
-n	Show line numbers
-v	Invert match (exclude pattern)
-E	Use extended regular expressions
Examples

Search for a word:

grep "error" logfile.txt

Case-insensitive search:

grep -i "error" logfile.txt

Show line numbers:

grep -n "error" logfile.txt

Count matches:

grep -c "error" logfile.txt

Exclude lines:

grep -v "INFO" logfile.txt

Recursive search in directory:

grep -r "error" /var/log

Extended regex:

grep -E "error|warning" logfile.txt

2 awk — print columns, field separator, patterns, BEGIN/END
   2. awk — Pattern Scanning and Text Processing

awk processes text line-by-line and column-by-column.

Basic Syntax
awk 'pattern { action }' file
Print Columns

Example file (data.txt):

Alice 25
Bob 30
Charlie 35

Print first column:

awk '{print $1}' data.txt

Print multiple columns:

awk '{print $1, $2}' data.txt
Field Separator

Specify delimiter with -F.

Example:

Alice,25
Bob,30
awk -F',' '{print $1}' data.csv
Pattern Matching
awk '/error/ {print}' logfile.txt
BEGIN and END Blocks

BEGIN runs before processing lines.
END runs after processing.

Example:

awk 'BEGIN {print "Start"} {print $1} END {print "Done"}' data.txt


3 sed — substitution, delete lines, in-place edit
    3. sed — Stream Editor

sed is used to edit text streams automatically.

Substitution

Replace text:

sed 's/error/warning/' file.txt

Replace all matches:

sed 's/error/warning/g' file.txt
Delete Lines

Delete line 3:

sed '3d' file.txt

Delete lines containing a pattern:

sed '/error/d' file.txt
In-place Editing

Modify file directly:

sed -i 's/error/warning/g' file.txt

4  cut — extract columns by delimiter
    4. cut — Extract Columns

cut extracts specific columns from text.

By Character Position
cut -c1-5 file.txt
By Delimiter

Example:

Alice,25,Engineer
Bob,30,Manager

Extract first column:

cut -d',' -f1 file.csv

Extract multiple columns:

cut -d',' -f1,3 file.csv

5 sort — alphabetical, numerical, reverse, unique
    5. sort — Sort Lines

sort arranges lines in order.

Alphabetical Sort
sort file.txt
Numerical Sort
sort -n numbers.txt
Reverse Sort
sort -r file.txt
Unique Sort
sort -u file.txt

6 uniq — deduplicate, count
    6. uniq — Remove Duplicate Lines

uniq removes consecutive duplicate lines.

⚠️ Usually used with sort.

Remove Duplicates
sort file.txt | uniq
Count Occurrences
sort file.txt | uniq -c

Example output:

3 error
5 warning
2 info

7 tr — translate/delete characters
    7. tr — Translate or Delete Characters

tr transforms characters from stdin.

Convert Lowercase to Uppercase
echo "hello" | tr 'a-z' 'A-Z'

Output:

HELLO
Delete Characters

Remove digits:

echo "abc123" | tr -d '0-9'

Output:

abc
Replace Characters
echo "hello world" | tr ' ' '_'

Output:

hello_world

8 wc — line/word/char count
    8. wc — Word Count

wc counts lines, words, and characters.

Flag	Description
-l	Line count
-w	Word count
-c	Character count
Examples

Line count:

wc -l file.txt

Word count:

wc -w file.txt

Character count:

wc -c file.txt

9 head / tail — first/last N lines, follow mode
    9. head and tail

These commands display beginning or end of files.

head — First Lines

Show first 10 lines:

head file.txt

Show first 5 lines:

head -n 5 file.txt
tail — Last Lines

Show last 10 lines:

tail file.txt

Show last 5 lines:

tail -n 5 file.txt
Follow Mode (Live Logs)

Monitor file updates:

tail -f logfile.txt

Useful for watching logs in real time.

Summary
Command	Purpose
grep	Search patterns
awk	Advanced text processing
sed	Stream editing
cut	Extract columns
sort	Sort lines
uniq	Remove duplicates
tr	Translate/delete characters
wc	Count lines/words/chars
head	First lines
tail	Last lines / live logs

Task 6: Useful Patterns and One-Liners
Include at least 5 real-world one-liners you find useful. Examples:

Find and delete files older than N days
1. Find and Delete Files Older Than N Days

This command finds files older than a specified number of days and deletes them.

Example

Delete .log files older than 7 days:

find /path/to/logs -type f -name "*.log" -mtime +7 -delete

Explanation:

Option	Meaning
find	Search files
-type f	Regular files
-name "*.log"	Match log files
-mtime +7	Older than 7 days
-delete	Remove the files

2 Count lines in all .log files
    2. Count Lines in All .log Files

Counts total lines across all log files in a directory.

wc -l *.log

To count total lines only:

cat *.log | wc -l

Example output:

120 system.log
350 access.log
470 total

3 Replace a string across multiple files
   3. Replace a String Across Multiple Files

Replace "localhost" with "127.0.0.1" in all .conf files.

sed -i 's/localhost/127.0.0.1/g' *.conf

Explanation:

Option	Meaning
sed	Stream editor
-i	Edit file in place
s/old/new/g	Replace all occurrences

4 Check if a service is running
   4. Check if a Service is Running

Check whether a service (example: nginx) is running.

systemctl is-active nginx

Alternative method:

ps aux | grep nginx | grep -v grep

If the process appears, the service is running.

5 Monitor disk usage with alerts
   5. Monitor Disk Usage with Alerts

Show disk usage and warn if usage exceeds 80%.

df -h | awk '$5+0 > 80 {print "Warning: Disk usage high on", $6}'

Example output:

Warning: Disk usage high on /home

6 Parse CSV or JSON from command line
    6. Parse CSV from Command Line

Extract specific columns from a CSV file.

Example CSV:

name,age,city
Alice,25,London
Bob,30,Paris

Command:

awk -F',' '{print $1, $3}' data.csv

Output:

Alice London
Bob Paris

7 Tail a log and filter for errors in real time
   Tail a Log and Filter Errors in Real Time

Monitor logs and show only error messages.

tail -f logfile.log | grep -i error

Explanation:

Command	Purpose
tail -f	Follow log file in real time
grep -i error	Show error messages

Example output:

ERROR: Database connection failed
ERROR: Invalid login attempt
Summary of Useful One-Liners
Task	Example Command
Delete old files	find /logs -name "*.log" -mtime +7 -delete
Count log lines	wc -l *.log
Replace text	sed -i 's/old/new/g' *.conf
Check service	systemctl is-active nginx
Disk usage alert	`df -h
Parse CSV	awk -F',' '{print $1,$2}' file.csv
Parse JSON	jq '.key' file.json
Monitor logs	`tail -f logfile

Task 7: Error Handling and Debugging
Document with examples:

1 Exit codes — $?, exit 0, exit 1
   1. Exit Codes

Every command in Linux returns an exit status code.

0 → Success

Non-zero (1–255) → Error or failure

The exit code of the last command can be accessed using:

$?
Example
ls file.txt
echo $?

If the file exists:

0

If it does not exist:

2
Manually Setting Exit Codes

You can explicitly exit a script with a status code.

exit 0   # success
exit 1   # generic error
Example Script
#!/bin/bash

if [ -f "data.txt" ]; then
    echo "File exists"
    exit 0
else
    echo "File not found"
    exit 1
fi


2 set -e — exit on error
   set -e (Exit on Error)

set -e makes the script immediately exit if any command fails.

Example
#!/bin/bash
set -e

echo "Start script"
mkdir testdir
cd testdir
rm non_existing_file   # script exits here
echo "This will not run"

Without set -e, the script would continue executing.

3 set -u — treat unset variables as error
  3. set -u (Treat Unset Variables as Errors)

set -u causes the script to fail if it uses an undefined variable.

Example
#!/bin/bash
set -u

echo "User is $USER_NAME"

Output:

bash: USER_NAME: unbound variable

This prevents silent bugs due to typos.

4 set -o pipefail — catch errors in pipes
    4. set -o pipefail (Catch Errors in Pipes)

Normally, in pipelines only the last command’s exit status is returned.

pipefail ensures the pipeline fails if any command in the pipe fails.

Example
#!/bin/bash
set -o pipefail

cat file.txt | grep "ERROR" | sort

If cat file.txt fails (file missing), the whole pipeline fails.

Without pipefail, the script might continue incorrectly.

5 set -x — debug mode (trace execution)
   5. set -x (Debug Mode)

set -x prints each command before execution, useful for debugging.

Example
#!/bin/bash
set -x

name="John"
echo "Hello $name"

Output:

+ name=John
+ echo Hello John
Hello John

This shows exactly what commands the script runs.

6 Trap — trap 'cleanup' EXIT
  6. trap (Cleanup on Exit)

trap allows running commands when the script exits or receives signals.

This is useful for cleanup tasks like removing temporary files.

Example
#!/bin/bash

cleanup() {
    echo "Cleaning up..."
    rm -f temp.txt
}

trap cleanup EXIT

echo "Creating temp file"
touch temp.txt

When the script exits:

Cleaning up...
Example: Robust Script with Error Handling
#!/bin/bash

set -euo pipefail

cleanup() {
    echo "Cleaning temporary files"
    rm -f temp.log
}

trap cleanup EXIT

echo "Starting script"

touch temp.log
grep "ERROR" server.log > temp.log

echo "Finished successfully"
What this script ensures

set -e → stop on failure

set -u → catch undefined variables

pipefail → detect pipeline failures

trap → cleanup even if script crashes

✅ Summary

Feature	Purpose
$?	Check exit code
exit N	Manually set exit code
set -e	Exit if a command fails
set -u	Fail on undefined variables
set -o pipefail	Catch failures in pipelines
set -x	Debug command execution
trap	Run cleanup on exit

Task 8: Bonus — Quick Reference Table
Create a summary table like this at the top of your cheat sheet:

Topic	Key Syntax	Example
Variable	VAR="value"	NAME="DevOps"
Argument	$1, $2	./script.sh arg1
If	if [ condition ]; then	if [ -f file ]; then
For loop	for i in list; do	for i in 1 2 3; do
Function	name() { ... }	greet() { echo "Hi"; }
Grep	grep pattern file	grep -i "error" log.txt
Awk	awk '{print $1}' file	awk -F: '{print $1}' /etc/passwd
Sed	sed 's/old/new/g' file	sed -i 's/foo/bar/g' config.txt
Task 8: Bonus — Quick Reference Table
Topic	Key Syntax	Example
Variable	VAR="value"	NAME="DevOps"
Print Variable	echo $VAR	echo $NAME
Script Argument	$1 $2 $3	./script.sh arg1 arg2
If Statement	if [ condition ]; then	if [ -f file.txt ]; then
If Else	if ... else ... fi	if [ $x -gt 5 ]; then echo "big"; else echo "small"; fi
For Loop	for i in list; do	for i in 1 2 3; do echo $i; done
While Loop	while [ condition ]; do	while [ $n -lt 5 ]; do echo $n; done
Function	name() { ... }	greet() { echo "Hi"; }
Command Substitution	$(command)	DATE=$(date)
Grep	grep pattern file	grep -i "error" log.txt
Awk	awk '{print $1}' file	awk -F: '{print $1}' /etc/passwd
Sed Replace	sed 's/old/new/g' file	sed -i 's/foo/bar/g' config.txt
Find Files	find path -name file	find /var/log -name "*.log"
Check Exit Code	$?	echo $?
Exit Script	exit N	exit 1
Debug Mode	set -x	set -x ./script.sh
Pipe Output	command1 | command2	cat file.txt | grep error
Count Lines	wc -l file	wc -l log.txt
Tail Logs	tail -f file	tail -f /var/log/syslog


