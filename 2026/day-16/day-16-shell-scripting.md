
Task 1: Your First Script

Create a file hello.sh
touch hellow.sh
vim hellow.sh

Add the shebang line #!/bin/bash at the top
#!/bin/bash


Print Hello, DevOps! using echo
echo hellow,devops!

Make it executable and run it
chmod +x hello.sh
./hello.sh

Document: What happens if you remove the shebang line?
If you remove the shebang (#!/bin/bash or similar) line, the script may not know which interpreter to use when executed directly.

If you run it like:

./script.sh

It may fail or run with the wrong shell.

But if you run it like:

bash script.sh

It will still work because you are explicitly telling the system to use bash.




Task 2: Variables

Create variables.sh with:
yes i created

A variable for your NAME
yes 

A variable for your ROLE (e.g., "DevOps Engineer")
Print: Hello, I am <NAME> and I am a ("devops engineer")

Try using single quotes vs double quotes — what's the difference?
Single Quotes (' ')

Everything inside is treated as literal text.

Variables are NOT expanded.

Example:

name="Priyanka"
echo 'Hello $name'

Output:

Hello $name

Double Quotes (" ")

Variables are expanded.

Special characters like $, `, \ are interpreted.

Example:

name="Priyanka"
echo "Hello $name"

Output:

Hello Priyanka

 Simple difference:
Single quotes = no variable expansion
Double quotes = variable expansion happens


Task 3: User Input with read

Create greet.sh that:
Asks the user for their name using read
Asks for their favourite tool
Prints: Hello <name>, your favourite tool is <tool>
#!/bin/bash

# Ask user for name
echo "Enter your name:"
read name

# Ask user for favourite tool
echo "Enter your favourite tool:"
read tool

# Print message
echo "Hello $name, your favourite tool is $tool"

task 4:

check_number.sh
#!/bin/bash

echo "Enter a number:"
read num

if [ "$num" -gt 0 ]; then
    echo "The number is positive."
elif [ "$num" -lt 0 ]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi


Task 5:


file_check.sh
#!/bin/bash

echo "Enter filename:"
read file

if [ -f "$file" ]; then
    echo "File exists."
else
    echo "File does not exist."
fi
server_check.sh
#!/bin/bash

service="nginx"

echo "Do you want to check the status? (y/n)"
read choice

if [ "$choice" = "y" ]; then
    systemctl is-active --quiet $service
    if [ $? -eq 0 ]; then
        echo "$service is active."
    else
        echo "$service is not active."
    fi
else
    echo "Skipped."
fi
