First, install `rsync` with the following command: `sudo apt install rsync`.

#### Creating a backup script
Navigate to your `root` directory to host the script (you can basically host it anywhere):
- `vim backup.sh`

Inside this script file, here is what you should be writing:
```
#!/bin/bash

# Directories to back up
SOURCE_DIRS=(
    "/path/to/your/first/directory"
    "/path/to/your/second/directory"
)

# Backup destination
BACKUP_DEST="/path/to/your/backup/directory"

# Date format for the backup filename
DATE=$(date +"%Y-%m-%d")

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DEST"

# Create a temporary directory for the backup
TEMP_BACKUP_DIR=$(mktemp -d)

# Loop through each source directory and copy it to the temporary backup directory
for DIR in "${SOURCE_DIRS[@]}"; do
    rsync -av --delete "$DIR" "$TEMP_BACKUP_DIR"
done

# Create a compressed tarball of the backup
tar -czf "$BACKUP_DEST/backup-$DATE.tar.gz" -C "$TEMP_BACKUP_DIR" .

# Remove the temporary backup directory
rm -rf "$TEMP_BACKUP_DIR"

```

The script creates a temporary directory (`mktemp -d`) to hold files before compression. Then, the `rsync` command copies the specified directories to the temporary backup directory. The `tar` command is used to create a compressed archive. Finally, the temporary backup directory is removed after the archive is created to save space.

![auto-backup](/images/backup-script.png)
#### Automating the backup
To automate the backup process, we will be setting up a `cron job`:
- open the crontab configuration with `crontab -e`
- add the following line to schedule the backup: `0 9 * * * /bin/bash /root/backup.sh >> /home/nox/backup/backup.log 2>$1`

The first part of this line will run the script everyday at 9am. The second part is there to monitor the backups by redirecting the output of the script to a log file. It will log both standard output and errors to `backup.log`.
