First, install `rsync` with the following command: `sudo apt install rsync`.

#### Creating a backup script
Navigate to your `home` directory to host the script (you can basically host it anywhere):
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

# Mount point for the backup partition
MOUNT_POINT="/path/to/mount/point"

# Device to mount (e.g., /dev/sda6)
DEVICE="/dev/sda6"

# Date format for the backup filename
DATE=$(date +"%Y-%m-%d")

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DEST"

# Mount the backup partition
if mount | grep "$MOUNT_POINT" > /dev/null; then
    echo "Backup partition is already mounted."
else
    echo "Mounting backup partition..."
    mount "$DEVICE" "$MOUNT_POINT"
    if [ $? -ne 0 ]; then
        echo "Failed to mount $DEVICE at $MOUNT_POINT. Exiting."
        exit 1
    fi
fi

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

# Unmount the backup partition
echo "Unmounting backup partition..."
umount "$MOUNT_POINT"
if [ $? -ne 0 ]; then
    echo "Failed to unmount $MOUNT_POINT. Please check manually."
    exit 1
fi

echo "Backup completed successfully."

```

The script creates a temporary directory (`mktemp -d`) to hold files before compression. Then, the `rsync` command copies the specified directories to the temporary backup directory. The `tar` command is used to create a compressed archive. Finally, the temporary backup directory is removed after the archive is created to save space.

![auto-backup](/images/backup-script.png)
#### Automating the backup
To automate the backup process, we will be setting up a `cron job`:
- open the crontab configuration with `crontab -e`
- add the following line to schedule the backup: `0 9 * * * /bin/bash /home/username/backup.sh >> /home/username/backup.log 2>$1`

The first part of this line will run the script everyday at 9am. The second part is there to monitor the backups by redirecting the output of the script to a log file. It will log both standard output and errors to `backup.log`.
