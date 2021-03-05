# Script to backup MySQL databases in a shared host environment

For several years, I had been using the [automysqlbackup](https://sourceforge.net/projects/automysqlbackup/) script to do nightly local backups of my website databases on my shared host. Unfortunately, my hosting company no longer supports automysqlbackup on shared hosting accounts. Running the script from my account home directory produced this error:

```
###### WARNING ######
Errors reported during AutoMySQLBackup execution.. Backup failed
Error log below..
./automysqlbackup: line 1058: /dev/fd/62: No such file or directory
```
Searching for a suitable replacement, I found [this script](https://grahamrpugh.com/2019/01/15/mysqldump-daily-weekly-monthly.html) published by Graham Pugh, which creates backups of MySQL databases on a daily, weekly and monthly basis using mysqldump. I made a couple of modifications to support using this script in a shared host account.

## Features

* Easily installed and run from the `$HOME` directory on a shared hosting account.
* Can be configured to run as a specific MySQL user.
* Finds and backs up muliple databases without having to be configured for each one.
* Saves a specified number of daily, weekly, and monthly backups of each database.

## Usage

Create a special MySQL user just for backups. Give this user limited permissions (SELECT, LOCK TABLES) on all the databases you wish to back up.

Save `mysqldump_cron.sh` and `mysqldump_defaults` in the `$HOME/bin` directory of your hosting account. Make the script executable.

Edit `mysqldump_defaults` with your database user credentials.

Create a new directory at `$HOME\backup`.

```
[client]
user=db_username
password=db_password
```

Test the script from the command line.

If everything works, set up a cron job to run the script daily.

`0 0 * * * /home/todd/bin/mysqldump_cron.sh >/dev/null 2>&1`

