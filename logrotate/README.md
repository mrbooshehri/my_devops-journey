# What is logrotate
Log rotation is a process of removing/storing old logs while not affecting the latest/newer logs.

# How it works?
Logrotate has a configuration file in which we can mention all the files
we want to rotate. It needs a time period unit i.e, daily, weekly,
monthly etc, and rotate count i.e, 3, 4, 5 for each rotation. Log files
are rotated count times before being removed. The default rotate count
is 0, which means old version of logs will be removed rather than
rotated. So, if your logs are being saved in a file named
my-application.log then after a rotation a new file will be created with
name my-application.1.log, and so on.

Main config file is under ```/etc/logrotate.conf```, but it's better
solution to separate your config as separate files and store them
under ```/etc/logrotate.d```

# Example
The following config will rotate a log file under ```/tmp``` named
```mylog.log```

```bash
/tmp/mylog.log {
daily
rotatate 1
compress
copytruncate
notifempty
missingok
maxsize 200M
# or  
#create 700 root appgroup
dateext
}
```

You can run scripts in logrotate after rotation

```bash
/tmp/mylog.log {
daily
rotatate 1
compress
compresscmd /bin/bzip2
compressext .bz2
copytruncate
notifempty
missingok
maxsize 200M
# or  
#create 700 root appgroup
dateext
postrotate
	systemctl restart myservice.service
	/home/bala/myscript.sh
endscript
}
```

## Explanation

1. ```/tmp/mylog.log``` demonstrate the path of the file we want to rotate
1. ```rotate 1``` store logs till 1 rotation, which means it will store
	 maximum 1 files of old logs when rotation hits, and will discard
	 oldest logs when there are already 1 files.
1. ```copytruncate``` it truncates the original log file to zero size in
	 place after creating a copy, instead of moving the old log file and
	 optionally creating a new one.
	 * It can be used when some program cannot be told to close its log
		 file and thus might continue writing (appending) to the previous
		 log file forever. Note that there is a very small time slice
		 between copying the file and truncating it, so some logging data
		 might be lost
1. ```missingok```, If the log file is missing, do not generate an
	 error, and move on the next file.
1. ```notifempty```, Do not rotate the log if it is empty
1. ```compress```, Old version of logs are compressed with gzip format.
1. ```maxsize 200M```, Rotate the log file if it exceeds 200 mb in size,
	 regardless of the rotation time unit.
1. ```daily```, Rotation should happen daily.
1. ```create```, it creates a file with 700 permission with user root
	 and group appgroup
1. ```dateext```, appends the filename with today’s date
