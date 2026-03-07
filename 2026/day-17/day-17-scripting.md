
donek 1: For Loop
Create for_loop.sh that:
Loops through a list of 5 fruits and prints each one
ans: touch for_loop.sh
vim for_loop.sh
Tas#!/bin/bash

for fruit in Apple Mango Banana Orange Grapes
do
  echo "Fruit: $fruit"
  chmod +x for_loop.sh
./for_loop.sh


Create count.sh that:
Prints numbers 1 to 10 using a for loop
touch count.sh
vim count.sh
#!/bin/bash

for i in {1..10}
do
  echo $i
done
chmod +x count.sh
./count.sh


Task 2: While Loop

Create countdown.sh that:
Takes a number from the user
touch countdown.sh
vim countdown.sh
#!/bin/bash

echo "Enter a number:"
read num

while [ $num -gt 0 ]
do
  echo $num
  num=$((num-1))
done
chmod +x countdown.sh
./countdown.sh


Counts down to 0 using a while loop
complited

Prints "Done!" at the end  
countdown.sh
#!/bin/bash

echo "Enter a number:"
read num

while [ $num -ge 0 ]
do
  echo $num
  num=$((num-1))
done

echo "Done!"
Example Output
Enter a number:
3
3
2
1
0
Done!

Task 3: Command-Line Arguments

Create greet.sh that:
Accepts a name as $1
Prints Hello, <name>!
If no argument is passed, prints "Usage: ./greet.sh "
 touch grret.sh
 vim grret.sh
 #!/bin/bash

if [ -z "$1" ]
then
  echo "Usage: ./greet.sh <priyanka>"
else
  echo "Hello, $1!"
fi
chmod +x grrte.sh
./grret.sh


Create args_demo.sh that:
Prints total number of arguments ($#)
Prints all arguments ($@)
Prints the script name ($0)
touch args_demo.sh
vim args_demo.sh
#!/bin/bash

echo "Total number of arguments: 3
echo "All arguments: apple mango banana
echo "Script name: ./ args_demo.sh
chmod +x args_demo.sh
./args_demo.sh

Task 4: Install Packages via Script
Create install_packages.sh that:
Defines a list of packages: nginx, curl, wget
Loops through the list
Checks if each package is installed (use dpkg -s or rpm -q)
Installs it if missing, skips if already present
Prints status for each package
touch install_packages.sh
vim install_packages.sh
chmod +x install_packages.sh
./install_packages.sh

Run as root: sudo -i or sudo su


Task 5: Error Handling
Create safe_script.sh that:
Uses set -e at the top (exit on error)
Tries to create a directory /tmp/devops-test
Tries to navigate into it
Creates a file inside
Uses || operator to print an error if any step fails
touch safe_script.sh
vim safe_script.sh
#!/bin/bash

# Exit immediately if any command fails
set -e

# Create directory
mkdir /tmp/devops-test || echo "Error: Failed to create directory"

# Navigate into directory
cd /tmp/devops-test || echo "Error: Failed to change directory"

# Create a file
touch testfile.txt || echo "Error: Failed to create file"

echo "Script completed successfully"
chmod +x safe_script.sh
./ safe_script.sh


Example:
mkdir /tmp/devops-test || echo "Directory already exists"

i will check

Modify your install_packages.sh to check if the script is being run as root — exit with a message if not.

touch install_packages.sh
vim install_packages.sh

#!/bin/bash

# Check if the script is run as root
if [ "$EUID" -ne 0 ]; then
    echo "Please run this script as root or using sudo."
    exit 1
fi

echo "Running as root..."

# Update package list
apt update

# Install packages
apt install -y git curl wget

echo "Packages installed successfully."
chmod +x install_packages.sh
 sudo ./install_packages.sh


