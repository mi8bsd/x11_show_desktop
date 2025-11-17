An open-source small tool written in GO for automating backups of FreeBSD jails managed with 
[Bastille](https://bastillebsd.org/).


## Requirements

- [FreeBSD](https://freebsd.org/)
- [BastilleBSD](https://bastillebsd.org/)
- ZFS filesystem for live backups.

## Disclaimer

I am a beginner programmer that wanted to automate the backup of all of my deployed jails on my servers.
I use GO as I find it to be the best programming language for my use case and abilities. Please use this 
tool as your own risk knowing that I probably will not have the time to work on this project full time on 
a regular basis.

## Features

- If no options are provided, it determines the list of current jails and backup them live in .gz format via ZFS snapshot
- If on UFS filesystem, it will warn you that jails can be only safely backup (start/stop)
- By default it removes backup files that are older than 2 days in /usr/local/bastille/backups/
- If specified with flags, it can optinally safely stop/start jails (for UFS filesystems) and remove
  backup files according to the retention period provided in number of days
- Logs activity in /var/log/jails.log

## TODO:
- Backup running jails only
- Do not start jails that were already stopped (current known bug)

## Installation

#### Git on FreeBSD
```shell
git clone https://github.com/mi8bsd/jails.git
cd jails
go build -o jails
chmod +x jails
mv jails /usr/local/bin/
```

#### Git on other platforms
```shell
git clone https://github.com/mi8bsd/jails.git
cd jails
env GOOS=freebsd GOARCH=amd64 go build -o jails
chmod +x jails
mv jails /usr/local/bin/
```

#### Crontab (every day at 1am)
```shell
0 1 * * * /usr/local/bin/jails
```

## Sample Usages

- jails
- jails safe 4
- jails live 9

## CMD options:

- **none**: hot backup jails and remove backups older than 2 days.
- **safe**: it safely stop jails before backing them up. Required if using UFS filesystem.
- **live**: hot backup jails without stopping them.
- **days**: number of days of data retention.
