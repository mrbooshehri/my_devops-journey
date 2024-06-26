# What is `logrotate`
Logrotate is a system utility that manages the automatic rotation,
compression, removal, and mailing of log files. It's widely used on
Unix-like operating systems (such as Linux) to help manage logs
generated by various applications and services. Logrotate allows
administrators to configure how log files should be handled
automatically, ensuring that disk space is not filled up by old log
entries.

## Key Features of Logrotate

1. **Automatic Rotation**: Logrotate can rotate log files automatically when they reach a certain size or after a specific time period has elapsed.
2. **Compression**: After rotating log files, Logrotate can compress them using gzip, bzip2, or another compression tool specified in its configuration.
3. **Emailing Logs**: If configured, Logrotate can mail the rotated log file(s) to one or more recipients.
4. **Multiple Configuration Files**: Logrotate supports multiple configuration files, allowing different settings for different groups of log files.
5. **Post/Pre-Rotation Scripts**: Users can specify scripts to run before or after log rotation, enabling custom actions like updating log file names or restarting services.
6. **Handling Multiple Log Files**: Logrotate can handle multiple log files per application, applying different configurations if needed.
7. **Security**: Logrotate can restrict access to log files based on permissions, ensuring that only authorized users can view or modify them.

## How Logrotate Works

Logrotate operates through a set of configuration files, typically located in `/etc/logrotate.conf` and in separate files under `/etc/logrotate.d/`. Each configuration file specifies the path to the log file(s), the conditions for rotation, and the actions to take upon rotation.

A basic configuration might look something like this:

```conf
/path/to/mylog {
    daily
    rotate 7
    compress
    missingok
    notifempty
    create 640 root adm
}
```

- `daily`: Rotate the log file every day.
- `rotate 7`: Keep seven days' worth of log files.
- `compress`: Compress (gzip, by default) the log files upon rotation.
- `missingok`: Do not throw an error if the log file is missing.
- `notifempty`: Do not rotate the log file if it is empty.
- `create 640 root adm`: Create new log files with specified permissions (in this case, 640) and ownership (root:adm) after rotation.

## Using Logrotate

To use Logrotate, you typically need to edit the configuration files to suit your needs. Once configured, Logrotate runs automatically according to the schedule defined in the configuration files. However, it can also be manually invoked from the command line to force rotation immediately.

For example, to manually rotate all log files according to their current configuration, you would use:

```bash
sudo logrotate /etc/logrotate.conf
```

Or, to rotate a specific log file or group of log files defined in a particular configuration file, you could specify that file:

```bash
sudo logrotate /etc/logrotate.d/myapp
```


# `logrotate` architecture

The architecture of Logrotate revolves around its core functionality and the way it interacts with its environment and configuration files. Understanding Logrotate's architecture involves looking at how it processes log files, applies rotations, and manages configurations across different environments. Here's a breakdown of its key components and how they work together:

## Core Components

1. **Configuration Files**: At the heart of Logrotate's operation are its configuration files. The main configuration file is typically found at `/etc/logrotate.conf`, which contains global options applicable to all log files managed by Logrotate. Additional configuration files are stored in `/etc/logrotate.d/`, where each file corresponds to a specific service or application, allowing for detailed customization of log rotation policies.

2. **Processing Engine**: When Logrotate runs, it reads the main configuration file and then iterates over each file in `/etc/logrotate.d/`, applying the global settings and any service-specific settings. This processing engine evaluates each log file against the criteria defined in the configuration files (e.g., file size, age) to determine if rotation is necessary.

3. **Rotation Logic**: For each log file that meets the criteria for rotation, Logrotate performs several steps:
   - **Backup**: Creates a backup of the current log file before rotating it. This ensures that there is always a recent copy available in case of errors during rotation.
   - **Compress**: Optionally compresses the rotated log file using gzip, bzip2, or another specified compression method.
   - **Create**: Creates a new log file, setting its initial size, owner, and group as specified in the configuration.
   - **Permissions**: Adjusts the permissions of the new log file as needed.
   - **Post/Pre-Rotation Hooks**: Executes scripts specified in the configuration for post-rotation tasks (e.g., restarting services) and pre-rotation checks (e.g., checking if a service is running).

4. **Logging and Notifications**: Logrotate can generate logs about its operations and send notifications via email or other means if configured. These features provide visibility into what Logrotate is doing and allow for quick responses to issues.

## Extensibility and Customization

- **Scripts**: Logrotate supports the execution of custom scripts before and after log rotation. This feature allows for extensive customization and automation beyond simple log file management, such as updating service configurations or restarting services.
- **Environment Variables**: Logrotate uses environment variables for some of its operations, providing a way to customize behavior without modifying the configuration files directly.
- **External Tools**: While Logrotate itself handles most aspects of log file management, it can be integrated with external tools for more advanced scenarios, such as offloading logs to remote servers or implementing sophisticated retention policies.

## Running Logrotate

Logrotate can be scheduled to run automatically at regular intervals using cron jobs or systemd timers, depending on the system's init system. This ensures that log files are managed regularly without manual intervention. Additionally, Logrotate can be invoked manually from the command line for testing or emergency rotations.

# Available Options

## Global Options

These options apply to all log files processed by Logrotate unless overridden by local configuration files.

- `abort`: Exit immediately if a rotation fails for any reason.
- `all`: Rotate all log files.
- `compress`: Use gzip to compress the log files.
- `delaycompress`: Delay compression of the log file to the next rotation cycle.
- `dateext`: Append a date to the end of the rotated file name.
- `dateformat`: Specify the format of the date appended to rotated filenames.
- `dateyesterday`: Like `dateext`, but append yesterday’s date instead of today’s.
- `debug`: Output debug information.
- `deltarotated`: Delete rotated log files older than a specified number of days.
- `force`: Force rotation even if it seems unnecessary.
- `include`: Include additional configuration files.
- `ignore`: Ignore files matching the given patterns.
- `mail`: Mail the output of the program to the specified addresses.
- `missingok`: Do not throw an error if the log file is missing.
- `missignok`: (Typo correction for `missingok`)
- `monthly`: Rotate the log files monthly (or whenever the 'month' option is set).
- `nocompress`: Never compress the log files.
- `no-create`: Do not create new log files.
- `notifempty`: Do not rotate the log file if it is empty.
- `olddir`: Move (rather than delete) rotated log files to the directory specified.
- `postrotate`: Specifies a script to execute after log rotation.
- `prerotate`: Specifies a script to execute before log rotation.
- `rotate`: Number of log files to keep.
- `sharedscripts`: Only execute the postrotate and prerotate scripts once, regardless of how many log files are rotated.
- `start`: Start date for log rotation.
- `stop`: Stop date for log rotation.
- `weekly`: Rotate the log files weekly (or whenever the 'week' option is set).
- `yearly`: Rotate the log files yearly (or whenever the 'year' option is set).
- `zero`: Zero out the log file after rotation.

## File Options

These options can be applied to individual log files or groups of log files within a configuration file.

- `copytruncate`: Truncate the original log file in place after creating a copy, instead of moving the old log file and optionally creating a new one.
- `create`: Create new log files with specified permissions and ownership.
- `create 0640 root adm`: Example usage specifying permissions, owner, and group.
- `dateext`: Similar to the global option but can be used to override it for specific files.
- `dateformat`: Similar to the global option but can be used to override it for specific files.
- `dateyesterday`: Similar to the global option but can be used to override it for specific files.
- `delaycompress`: Similar to the global option but can be used to override it for specific files.
- `deltarotated`: Similar to the global option but can be used to override it for specific files.
- `exclude`: Exclude files matching the given patterns.
- `firstinstance`: Only rotate the log file if it already exists.
- `missingok`: Similar to the global option but can be used to override it for specific files.
- `notifempty`: Similar to the global option but can be used to override it for specific files.
- `olddir`: Similar to the global option but can be used to override it for specific files.
- `postrotate`: Similar to the global option but can be used to override it for specific files.
- `prerotate`: Similar to the global option but can be used to override it for specific files.
- `rotate`: Similar to the global option but can be used to override it for specific files.
- `size`: Rotate the log file when it grows too large, specified in kilobytes.
- `size 500K`: Example usage specifying the size limit in kilobytes.
- `weekly`: Similar to the global option but can be used to override it for specific files.
- `yearly`: Similar to the global option but can be used to override it for specific files.


# Logrotate Strategies

Logrotate supports various rotation strategies, each suited to different types of log files and use cases. Understanding these strategies can help you tailor log management to fit your specific needs. Below, I'll outline the primary rotation strategies, along with typical use cases and examples of how to implement them in a Logrotate configuration.

## 1. Size-based Rotation

**Use Case**: Ideal for log files that grow over time, such as web server access logs or application error logs.

**Implementation**:
```conf
/path/to/access.log {
    daily
    rotate 7
    size 10M
    missingok
    notifempty
    compress
    delaycompress
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
In this example, the log file will be rotated when it reaches 10MB in size, keeping up to 7 rotated files. The `delaycompress` option ensures that the first rotated file is compressed, avoiding potential issues with programs trying to read the log file while it's being written to.

## 2. Time-based Rotation

**Use Case**: Suitable for logs that need to be archived daily, weekly, monthly, or yearly, such as system logs or application status logs.

**Implementation**:
```conf
/path/to/system.log {
    daily
    rotate 30
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
Here, the log file is rotated daily, and up to 30 rotated files are kept. This strategy is useful for logs that accumulate data over time but do not necessarily grow in size.

## 3. Count-based Rotation

**Use Case**: Useful for logs that don't necessarily grow in size but still need to be limited in number, such as audit logs or security logs.

**Implementation**:
```conf
/path/to/security.log {
    rotate 14
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
In this setup, the log file is rotated every time it reaches the maximum count (14 in this case), ensuring that no more than 14 rotated files are kept.

## 4. Date Extension

**Use Case**: Adds a date extension to the rotated log file name, preventing conflicts between rotated files and making it easier to identify when each file was created.

**Implementation**:
```conf
/path/to/error.log {
    dateext
    dateformat -%Y%m%d%s
    rotate 12
    missingok
    notifempty
    compress
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
This configuration adds a timestamp to the rotated log file name, following the format `-YYYYMMDDHHMMSS`, ensuring unique file names for each rotation.

## 5. Missing Log Handling

**Use Case**: Ensures that Logrotate doesn't fail if a log file is missing, which can happen if the log file is deleted or moved outside of Logrotate's normal rotation cycle.

**Implementation**:
```conf
/path/to/myservice.log {
    missingok
    rotate 5
    compress
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
By including `missingok`, Logrotate will skip the file if it's missing, preventing errors during the rotation process.

## 6. Compression

**Use Case**: Reduces disk space usage by compressing rotated log files, especially useful for long-term storage or when dealing with large log files.

**Implementation**:
```conf
/path/to/application.log {
    rotate 7
    compress
    missingok
    notifempty
    sharedscripts
    postrotate
        /path/to/your/script.sh
    endscript
}
```
Adding `compress` instructs Logrotate to compress the rotated log files using gzip by default.


# `logrotate` huge files

Rotating very large log files, such as those exceeding 100GB, presents unique challenges due to the potential impact on system performance and the risk of interrupting critical processes that rely on these logs. To rotate such large log files without interruption, careful planning and configuration are essential. Here's a strategy tailored for this scenario:

## 1. Pre-Rotation Check

First, ensure that the process writing to the log file is healthy and that the system has enough resources (disk space, CPU, memory) to handle the rotation smoothly. This might involve temporarily increasing disk space or adjusting resource limits for the process generating the log.

## 2. Use `copytruncate`

The `copytruncate` option allows Logrotate to make a copy of the current log file and then truncate the original file in place. This method avoids locking the file during the rotation process, minimizing the impact on the writing process.

```conf
/path/to/large-log.log {
    daily
    rotate 7
    copytruncate
    compress
    missingok
    notifempty
    sharedscripts
    postrotate
        /path/to/post-rotate-script.sh
    endscript
}
```

## 3. Large File Support

Ensure that the filesystem and the underlying hardware support the size of the log files. Some filesystems have limitations on the maximum file size they can handle efficiently. For example, ext4 supports files up to 16TB, but performance degrades significantly above 100GB.

## 4. Adequate Disk Space

Before attempting to rotate a very large log file, ensure there is sufficient disk space available. Lack of space can cause the rotation to fail or lead to data loss. Consider implementing a monitoring solution to alert you when disk space is low.

## 5. Parallel Execution

If you have multiple large log files to rotate, consider using the `parallel` option to rotate them simultaneously. This can reduce the overall downtime compared to rotating them sequentially.

```conf
{
    # Common options for all log files
    missingok
    notifempty
    compress
    sharedscripts
}

# Specific configuration for each log file
/path/to/large-log1.log {
    daily
    rotate 7
    copytruncate
    postrotate
        /path/to/post-rotate-script1.sh
    endscript
}

/path/to/large-log2.log {
    daily
    rotate 7
    copytruncate
    postrotate
        /path/to/post-rotate-script2.sh
    endscript
}

# And so on for other log files...
```

## 6. Post-Rotation Script

Implement a post-rotation script that can handle any necessary cleanup or restarts of dependent services. This script should be idempotent and able to run multiple times without causing issues.

## 7. Testing

Before applying changes to production, thoroughly test the rotation process in a staging environment. Ensure that the rotation occurs smoothly and that no unintended side effects occur, such as service disruptions or data loss.

## 8. Monitoring and Alerts

Set up monitoring and alerts for the log files and the system's health metrics. This can help you quickly detect and respond to issues related to log rotation or system performance.


# Avoid locking logfile

When Logrotate compresses rotated log files, it typically locks the file to prevent simultaneous access, which can disrupt processes that write to the log file. However, there are strategies to mitigate the impact of this locking mechanism, especially for critical applications that cannot tolerate downtime during log rotation. One common approach is to use the `copytruncate` option, which copies the log file and then truncates it, avoiding the need to lock the file during compression. Another approach involves custom scripting to handle the compression externally. Here's how you can implement these strategies:

### Using `copytruncate`

The `copytruncate` option makes a copy of the log file and then truncates the original file in place. This method avoids locking the file during the rotation process, which can be beneficial for large log files or when high availability is crucial.

```conf
/path/to/your-large-log.log {
    daily
    rotate 7
    copytruncate
    compress
    missingok
    notifempty
    sharedscripts
    postrotate
        /path/to/post-rotate-script.sh
    endscript
}
```

However, note that `copytruncate` might not be suitable for all situations, especially if the process writing to the log file does not handle truncated files well or if the log file is very large.

### External Compression Script

Another approach is to handle the compression outside of Logrotate entirely. This can be done by creating a custom script that compresses the log file after it has been rotated and then tells Logrotate to skip the compression step.

1. **Custom Post-Rotate Script**: Write a script that compresses the rotated log file using your preferred compression tool (e.g., gzip, bzip2). Place this script in the `postrotate` section of your Logrotate configuration.

```sh
#!/bin/bash
# postrotate_script.sh
find /path/to/rotated/logs -type f -name "*.log" -mtime +0 | xargs -I {} gzip {}
```

2. **Modify Logrotate Configuration**: In your Logrotate configuration, remove the `compress` option and ensure the `postrotate` script is correctly referenced.

```conf
/path/to/your-large-log.log {
    daily
    rotate 7
    missingok
    notifempty
    sharedscripts
    postrotate
        /path/to/post-rotate-script.sh
    endscript
}
```

3. **Make the Script Executable**: Ensure your script is executable by running `chmod +x /path/to/post-rotate-script.sh`.

### Considerations

- **Testing**: Thoroughly test your configuration in a non-production environment to ensure that the log rotation and compression process works as expected without disrupting your applications.
- **Resource Management**: Be mindful of the resources required for compressing large log files. Ensure that your system has enough disk space and that the compression process does not negatively impact system performance.
- **Monitoring**: Implement monitoring and alerting mechanisms to track the success of log rotations and to be notified of any issues that arise.

By using either the `copytruncate` option or an external compression script, you can avoid having Logrotate lock the log file during compression, thus minimizing the impact on applications that write to these logs.
