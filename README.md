# Backup Script

A bash script that creates compressed backups of recently modified files in a target directory.

## Overview

This script creates a timestamped tar.gz backup file containing all files from a target directory that have been modified within the last 24 hours. The backup is then moved to a specified destination directory.

## Features

- ✅ Validates command-line arguments
- ✅ Checks directory paths exist
- ✅ Creates timestamped backup files
- ✅ Only backs up recently modified files (last 24 hours)
- ✅ Compresses backups using gzip
- ✅ Moves completed backups to destination directory
- ✅ Error handling for directory navigation

## Usage

```bash
./backup.sh <target_directory> <destination_directory>
```

### Parameters

- `target_directory`: The directory containing files to backup
- `destination_directory`: The directory where the backup file will be stored

### Example

```bash
./backup.sh /home/user/documents /home/user/backups
```

This will:
1. Scan `/home/user/documents` for files modified in the last 24 hours
2. Create a compressed backup file named `backup-<timestamp>.tar.gz`
3. Move the backup file to `/home/user/backups`

## How It Works

### Step-by-Step Process

1. **Argument Validation**: Checks that exactly 2 arguments are provided
2. **Directory Validation**: Verifies both directories exist
3. **Variable Setup**: Stores directory paths and generates timestamp
4. **Path Resolution**: Gets absolute paths for reliable navigation
5. **File Discovery**: Finds files modified within the last 24 hours
6. **Backup Creation**: Creates compressed tar.gz archive
7. **File Movement**: Moves backup to destination directory

### File Selection Logic

The script uses the following criteria to select files for backup:
- Files must be in the target directory (not subdirectories)
- Files must have been modified within the last 24 hours
- Uses `date -r` command to check file modification times

## Output

The script provides feedback during execution:

```
Target Directory: /path/to/target
Destination Directory: /path/to/destination
My ARRAY file1.txt file2.log file3.doc
```

The final backup file will be named in the format:
```
backup-<unix_timestamp>.tar.gz
```

## Requirements

- Bash shell
- Standard Unix utilities: `date`, `tar`, `pwd`, `cd`, `mv`
- Read permissions on target directory
- Write permissions on destination directory

## Error Handling

The script handles several error conditions:

- **Incorrect number of arguments**: Displays usage message and exits
- **Invalid directory paths**: Displays error message and exits
- **Directory navigation failures**: Uses `|| exit` to terminate on `cd` failures

## Technical Details

### Key Variables

- `currentTS`: Unix timestamp when script runs
- `yesterdayTS`: Timestamp from 24 hours ago
- `backupFileName`: Generated backup file name
- `toBackup`: Array of files to include in backup
- `origAbsPath`: Original working directory
- `destDirAbsPath`: Absolute path of destination directory

### File Processing

The script uses a bash array to collect files for backup:

```bash
declare -a toBackup
for file in *; do
  if [[ $(date -r "$file" +%s) -gt "$yesterdayTS" ]]; then
    toBackup+=("$file")
  fi
done
```

## Installation

1. Save the script as `backup.sh`
2. Make it executable:
   ```bash
   chmod +x backup.sh
   ```
3. Run with appropriate arguments

## Limitations

- Only processes files in the target directory (not subdirectories)
- Requires both directories to exist before running
- Uses 24-hour window based on script execution time
- Does not handle symbolic links specially

## License

This script is provided as-is for educational and practical use.

## Contributing

Feel free to enhance this script by adding features like:
- Recursive directory processing
- Configurable time windows
- Logging capabilities
- Email notifications
- Automated scheduling integration
