
Task 1: Basic Functions

Create functions.sh with:
A function greet that takes a name as argument and prints Hello, <name>!
A function add that takes two numbers and prints their sum
Call both functions from the script
touch function.sh
vim function.sh
#!/bin/bash

# Function to greet a user
greet() {
    name=$1
    echo "Hello, $name!"
}

# Function to add two numbers
add() {
    num1=$1
    num2=$2
    sum=$((num1 + num2))
    echo "Sum: $sum"
}

# Calling the functions
greet "Rohit"
add 5 7
chmod +x function.sh
./function.sh

Task 2: Functions with Return Values

Create disk_check.sh with:
A function check_disk that checks disk usage of / using df -h
A function check_memory that checks free memory using free -h
A main section that calls both and prints the results
touch disk_check.sh
vim disk_check.sh
#!/bin/bash

# Function to check disk usage
check_disk() {
    echo "Disk Usage for / :"
    df -h /
}

# Function to check memory usage
check_memory() {
    echo "Memory Usage:"
    free -h
}

# Main section
echo "===== System Resource Check ====="
check_disk
echo ""
check_memory
chmod +x disk_check.sh
./disk_check.sh


Task 3: Strict Mode — set -euo pipefail

Create strict_demo.sh with set -euo pipefail at the top
Try using an undefined variable — what happens with set -u?
Try a command that fails — what happens with set -e?
Try a piped command where one part fails — what happens with set -o pipefail?
Document: What does each flag do?

set -e →
set -u →
set -o pipefail →
touch script_demo.sh
vim script_demo.sh
#!/bin/bash

# Enable strict mode
set -euo pipefail

echo "Starting strict mode demo..."

# --- set -u example ---
echo "Using an undefined variable:"
echo "$UNDEFINED_VAR"

# --- set -e example ---
echo "Running a command that fails:"
ls /nonexistent_directory

# --- set -o pipefail example ---
echo "Testing pipefail:"
cat nonexistentfile.txt | grep "hello"

echo "Script completed"
chmod +x script_demo.sh
./ script_demo.sh
Documentation (Required Part)

set -e →
Exit the script immediately if any command returns a non-zero (error) status.

set -u →
Treat undefined variables as errors and exit the script.

set -o pipefail →
If any command in a pipeline fails, the whole pipeline fails.


Task 4: Local Variables

Create local_demo.sh with:
A function that uses local keyword for variables
Show that local variables don't leak outside the function
Compare with a function that uses regular variables
touch local_demo.sh
vim local_demo.sh
#!/bin/bash

# Function using local variables
local_function() {
    local message="I am a LOCAL variable"
    echo "Inside local_function: $message"
}

# Function using regular (global) variables
global_function() {
    message="I am a GLOBAL variable"
    echo "Inside global_function: $message"
}

echo "Calling local_function..."
local_function

echo "Outside local_function: $message"

echo ""
echo "Calling global_function..."
global_function

echo "Outside global_function: $message"
chmod +x local_demo.sh
./local_demo.sh

Task 5: Build a Script — System Info Reporter

Create system_info.sh that uses functions for everything:

A function to print hostname and OS info
A function to print uptime
A function to print disk usage (top 5 by size)
A function to print memory usage
A function to print top 5 CPU-consuming processes
A main function that calls all of the above with section headers
Use set -euo pipefail at the top
Output should look clean and readable.
touch system_info.sh
vim system_info.sh
#!/bin/bash

# Enable strict mode
set -euo pipefail

print_header() {
    echo "===================================="
    echo "$1"
    echo "===================================="
}

system_info() {
    print_header "SYSTEM INFORMATION"
    echo "Hostname: $(hostname)"
    echo "OS: $(uname -a)"
    echo ""
}

uptime_info() {
    print_header "SYSTEM UPTIME"
    uptime -p
    echo ""
}

disk_usage() {
    print_header "TOP 5 DISK USAGE (Directories)"
    du -h / 2>/dev/null | sort -hr | head -n 5
    echo ""
}

memory_usage() {
    print_header "MEMORY USAGE"
    free -h
    echo ""
}

top_cpu_processes() {
    print_header "TOP 5 CPU-CONSUMING PROCESSES"
    ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6
    echo ""
}

main() {
    system_info
    uptime_info
    disk_usage
    memory_usage
    top_cpu_processes
}

main
chmod +x system_info.sh
./system_info.sh



Happy Learning! TrainWithShubham
