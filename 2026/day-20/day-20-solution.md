
Task 1: Input and Validation

Your script should:

Accept the path to a log file as a command-line argument
Exit with a clear error message if no argument is provided
Exit with a clear error message if the file doesn't exist
touch log_analyzer.sh
vim log_analyzer.sh
#!/bin/bash

# Check if argument is provided
if [ $# -eq 0 ]; then
    echo "Error: No log file provided."
    echo "Usage: $0 <log_file_path>"
    exit 1
fi

LOG_FILE="$1"

# Check if file exists
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

echo "Log file '$LOG_FILE' loaded successfully."
./log_analyzer.sh
./log_analyzer.sh /var/log/syslog
./log_analyzer.sh fake.log
cat log_analyzer.sh

Task 2: Error Count

Count the total number of lines containing the keyword ERROR or Failed
Print the total error count to the console
vim log_analyzer.sh
./log_analyzer.sh /var/log/syslog
./log_analyzer.sh /var/log/syslog
grep -Ei "ERROR|Failed" /var/log/syslog


Task 3: Critical Events

Search for lines containing the keyword CRITICAL
Print those lines along with their line number
Example output:

--- Critical Events ---
Line 84: 2025-07-29 10:15:23 CRITICAL Disk space below threshold
Line 217: 2025-07-29 14:32:01 CRITICAL Database connection lost
Total errors found: 12

Critical events found:
----------------------
45:CRITICAL: Database connection lost
103:CRITICAL: Disk space exhausted
208:CRITICAL: Service crashed unexpectedly


Task 4: Top Error Messages
Extract all lines containing ERROR
Identify the top 5 most common error messages
Display them with their occurrence count, sorted in descending order
Example output:

--- Top 5 Error Messages ---
45 Connection timed out
32 File not found
28 Permission denied
15 Disk I/O error
9  Out of memory
| Command           | Purpose                   |
| ----------------- | ------------------------- |
| `grep -i "ERROR"` | Extract error lines       |
| `sort`            | Prepare data for counting |
| `uniq -c`         | Count duplicates          |
| `sort -nr`        | Sort by highest count     |
| `head -5`         | Show top 5                |




Task 5: Summary Report
Generate a summary report to a text file named log_report_<date>.txt (e.g., log_report_2026-02-11.txt). The report should include:

Date of analysis
Log file name
Total lines processed
Total error count
Top 5 error messages with their occurrence count
List of critical events with line numbers
touch log_report_2026-02-11.txt
vim  log_report_2026-02-11.txt
# Task 5: Generate Summary Report

DATE=$(date +%F)
REPORT_FILE="log_report_$DATE.txt"

TOTAL_LINES=$(wc -l < "$LOG_FILE")
ERROR_COUNT=$(grep -Eic "ERROR|Failed" "$LOG_FILE")

{
echo "Log Analysis Report"
echo "==================="
echo "Date of Analysis: $DATE"
echo "Log File: $LOG_FILE"
echo "Total Lines Processed: $TOTAL_LINES"
echo "Total Error Count: $ERROR_COUNT"
echo ""

echo "--- Top 5 Error Messages ---"
grep -i "ERROR" "$LOG_FILE" | sort | uniq -c | sort -nr | head -5
echo ""

echo "--- Critical Events ---"
grep -ni "CRITICAL" "$LOG_FILE"

} > "$REPORT_FILE"

echo "Report generated: $REPORT_FILE"
./log_analyzer.sh /var/log/syslog
cat log_report_2026-02-11.txt


Task 6 (Optional): Archive Processed Logs
Add a feature to:

Create an archive/ directory if it doesn't exist
Move the processed log file into archive/ after analysis
Print a confirmation message

Total errors found: 27

Critical events found:
----------------------
45:CRITICAL: Database connection lost

--- Top 5 Error Messages ---
12 ERROR Connection refused
8 ERROR File not found
5 ERROR Permission denied

Report generated: log_report_2026-02-11.txt
Log file moved to archive/

project/
│
├── log_analyzer.sh
├── log_report_2026-02-11.txt
│
└── archive/
      └── app.log



