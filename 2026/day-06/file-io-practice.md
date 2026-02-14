Creating a file
touch notes.txt

Writing text to a file Appending new lines Reading the file back, Keep it basic and repeatable.
echo "First line" > notes.txt
echo "Second line" >> notes.txt
echo "Third line" >> notes.txt

Use head and tail to read parts of the file

head -n 2 notes. txt
tail -n 2 notes. txt

Use tee once to write and display at the same time
echo "my husbend name is rohit" | tee -a notes.txt

