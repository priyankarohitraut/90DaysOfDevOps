
Task 1: Log Rotation Script

Create log_rotate.sh that:

Takes a log directory as an argument (e.g., /var/log/myapp)
Compresses .log files older than 7 days using gzip
Deletes .gz files older than 30 days
Prints how many files were compressed and deleted
Exits with an error if the directory doesn't exist
touch log_rotate.sh
vim log_rotate.sh
#!/bin/bash

set -euo pipefail

# Check if argument is provided
if [ $# -ne 1 ]; then
    echo "Usage: $0 <log_directory>"
    exit 1
fi

LOG_DIR="$1"

# Check if directory exists
if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory $LOG_DIR does not exist."
    exit 1
fi

compressed_count=0
deleted_count=0

# Compress .log files older than 7 days
for file in $(find "$LOG_DIR" -type f -name "*.log" -mtime +7); do
    gzip "$file"
    ((compressed_count++))
do

# Delete .gz files older than 30 days
for file in $(find "$LOG_DIR" -type f -name "*.gz" -mtime +30); do
    rm "$file"
    ((deleted_count++))
done

echo "Log rotation completed."
echo "Files compressed: $compressed_count"
echo "Files deleted: $deleted_count"
chmod +x log_rotate.sh
./log_rotate.sh

Task 2: Server Backup Script

Create backup.sh that:

Takes a source directory and backup destination as arguments
Creates a timestamped .tar.gz archive (e.g., backup-2026-02-08.tar.gz)
Verifies the archive was created successfully
Prints archive name and size
Deletes backups older than 14 days from the destination
Handles errors — exit if source doesn't exist
touch backup.sh
vim backup.sh
#!/bin/bash

set -euo pipefail

# Check arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_directory> <backup_destination>"
    exit 1
fi

SOURCE="$1"
DEST="$2"

# Check if source exists
if [ ! -d "$SOURCE" ]; then
    echo "Error: Source directory does not exist."
    exit 1
fi

# Create destination if it doesn't exist
mkdir -p "$DEST"

# Create timestamp
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")

ARCHIVE="backup-$TIMESTAMP.tar.gz"
ARCHIVE_PATH="$DEST/$ARCHIVE"

# Create backup
tar -czf "$ARCHIVE_PATH" -C "$SOURCE" .

# Verify backup was created
if [ -f "$ARCHIVE_PATH" ]; then
    SIZE=$(du -h "$ARCHIVE_PATH" | cut -f1)
    echo "Backup created successfully."
    echo "Archive: $ARCHIVE"
    echo "Size: $SIZE"
else
    echo "Error: Backup failed."
    exit 1
fi

# Delete backups older than 14 days
DELETED=$(find "$DEST" -type f -name "backup-*.tar.gz" -mtime +14 -delete -print | wc -l)

echo "Old backups deleted: $DELETED"
chmod +x backup.sh
./backup.sh


Task 3: Crontab

Read: crontab -l — what's currently scheduled?
Understand cron syntax:
* * * * *  command
│ │ │ │ │
│ │ │ │ └── Day of week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
Write cron entries (in your markdown, don't apply if unsure) for:
Run log_rotate.sh every day at 2 AM
Run backup.sh every Sunday at 3 AM
Run a health check script every 5 minutes
service cron status
cronta -e
# Log rotation daily
0 2 * * * /home/user/scripts/log_rotate.sh /var/log/myapp

# Weekly backup
0 3 * * 0 /home/user/scripts/backup.sh /data /backups

# Health check
*/5 * * * * /home/user/scripts/health_check.sh
crontab -l

Task 4: Combine — Scheduled Maintenance Script

Create maintenance.sh that:

Calls your log rotation function
Calls your backup function
Logs all output to /var/log/maintenance.log with timestamps
Write the cron entry to run it daily at 1 AM
touch mantenace.sh
vim maintenace.sh
#!/bin/bash

set -euo pipefail

LOG_FILE="/var/log/maintenance.log"

# Function to print timestamped log messages
log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1"
}

# Function for log rotation
rotate_logs() {
    LOG_DIR="/var/log/myapp"

    if [ ! -d "$LOG_DIR" ]; then
        log "ERROR: Log directory $LOG_DIR does not exist"
        exit 1
    fi

    COMPRESSED=$(find "$LOG_DIR" -type f -name "*.log" -mtime +7 -exec gzip {} \; -print | wc -l)
    DELETED=$(find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -delete -print | wc -l)

    log "Log rotation completed"
    log "Compressed files: $COMPRESSED"
    log "Deleted files: $DELETED"
}

# Function for backup
run_backup() {
    SOURCE="/home/rohit/data"
    DEST="/home/rohit/backups"

    if [ ! -d "$SOURCE" ]; then
        log "ERROR: Source directory $SOURCE does not exist"
        exit 1
    fi

    mkdir -p "$DEST"

    TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
    ARCHIVE="$DEST/backup-$TIMESTAMP.tar.gz"

    tar -czf "$ARCHIVE" -C "$SOURCE" .

    if [ -f "$ARCHIVE" ]; then
        SIZE=$(du -h "$ARCHIVE" | cut -f1)
        log "Backup created: $ARCHIVE"
        log "Backup size: $SIZE"
    else
        log "ERROR: Backup failed"
        exit 1
    fi

    OLD=$(find "$DEST" -type f -name "backup-*.tar.gz" -mtime +14 -delete -print | wc -l)
    log "Old backups deleted: $OLD"
}

# Main function
main() {
    log "===== Maintenance Started ====="

    rotate_logs
    run_backup

    log "===== Maintenance Completed ====="
}

# Run and log output
main >> "$LOG_FILE" 2>&1
chmod +x maintenace.sh
./maintenace.sh
crontab -e



