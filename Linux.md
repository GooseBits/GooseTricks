# Contents
[[_TOC_]]

# Init Systems and Service Management

| Action       | SysVInit | SystemD | OpenRC |
| ------------- |----------|---------|---------|
| Start/Stop/Restart | `service <SERVICE> start/stop/restart` | `systemctl stop/start/restart <SERVICE>` | `rc-service <SERVICE> start/stop/restart` |
| Get service status | `service <SERVICE> status` | `systemctl status <SERVICE>` | `rc-service <SERVICE> status` |
| Enable for service for boot | Old RHEL/CentOS: `chkconfig <SERVICE> on`<br>Old Ubuntu: `update-rc.d <SERVICE> defaults` | `systemctl enable <SERVICE>` | `rc-update add <SERVICE>` |
| Notes | |Systemctl uses `journald` for logs, view with `journalctl`.||

# Mandatory Access Control Systems

| Command       | SELinux  | AppArmor |
| ------------- |----------|----------|
| Status | `sestatus` | `aa-status` |
| Logs | `/var/log/audit/audit.log` or `/var/log/audit.log` | `/var/log/kern.log` |
| To Disable | `setenforce 0` or for permanently edit `/etc/selinux/config`, set `SELINUX=` to `disabled` | Works as service, `sudo systemctl stop/disable apparmor` |

# Files

## Finding

| Command       | Description |
| ------------- |----------|
| `grep -ri <PATTERN> <START_DIR>` | Find files with text |
| `find / -name "<GLOB_PATTERN>"` | Find file with name |
| `find <DIR> -user root -perm -4000 -exec ls -ldb {} \; ` | Find setuid files |
| `find / -perm u+w,g+w,o+w` | Find world-writable |
| `find / -uid 0 -perm u+w,g+w,o+w` | Find world-writable owned by UID 0 (alternatives below) |
| `find / -type f -user root -perm /o=w` | Find world-writable files owned by root |
| `find / -type d -user root -perm /o=w` | Find world-writable directories owned by root |

## Mounting

| Command       | Description |
| ------------- |----------|
| `mount -t cifs -o username=user,password=pass,domain=blah //<ADDRESS>/share-name /mnt/cifs` | Mount SMB |
| `mount -o loop <FILE>.iso /mnt/mount_point` | Mount ISO file |


## File Management

| Command       | Description |
| ------------- | ---------- |
| Setuid file | `chmod 4000 <FILE>` or `chmod u+s <FILE>`. Beware FS with `nosetuid` flag set. |
| Get top 10 file lines | `head -n 10 <FILE>` |
| Get top 10 bytes lines | `head -c 10 <FILE>` |
| Get last 10 file lines | `tail -n 10 <FILE>` |
| Get last 10 bytes lines | `head -c 10 <FILE>` |
| Follow file and print updates as they are written | `tail -f <FILE>` |

## Cron (Scheduled tasks)
| Command       | Description |
| ------------- | ---------- |
| `crontab -e` | Edit crontab for current user |
| `crontab -u <USER> -e` | Edit crontab for <USER> |
| `crontab -l` | List crontabs |

### Cron entries
#### User Crontab
```
<MIN> <HOUR> <DAY_OF_MON> <MON> <DAY_OF_WEEK> <COMMAND>
```
#### Global cron (`/etc/cron*`)
```
<MIN> <HOUR> <DAY_OF_MON> <MON> <DAY_OF_WEEK> <USER> <COMMAND>
```

| Unit | Range | 
| -------| ----- |
| `<MIN>` | 0-59 |
| `<HOUR>` | 0-23 |
| `<DAY_OF_MON>` | 1-31 |
| `<MON>` | 1 - 12 |
| `<DAY_OF_WEEK>` | 0 - 6 |

### Cron Special Characters
| Command       | Description | Example |
| ------------- | ---------- | -------|
| `/<NUM>` | Every `<NUM>` units | `*/15 * ...` every 15 minutes, not always supported. |
| `<NUM1>,<NUM2>` | On `<NUM1>` and `<NUM2>` | `* 0,12 ...` On hour 0 and 12 |
| `*` | Any | |

`cron.deny` Denies users for cron.

# Command Line Website Interactions
## curl
| Option       | Description |
| ------------- | ---------- |
| `-X<TYPE>` | HTTP Type (POST, GET, PUT, DELETE) |
| `-F <KEY>=<DATA>` | Post form data, KEY is field name, DATA is posted data |
| `-F <KEY>=@<FILE>` | Post form file, KEY is field name, FILE is the file, note the `@` |
| `--cacert <PATH>` | Set CA cert for TLS verification |
| `-k/--insecure` | Ignore SSL verification issues |
| `-b/--cookie "NAME1=VALUE1; NAME2=VALUE2"` | Set Cookie |
| `-d/--data <DATA>` | Post raw data |
| `A/--user-agent <AGENT_STRING>` | Set user agent |
| `-u <USER>` | Use HTTP auth, prompts for password |
| `-u <USER>:<PASSWORD>` | Use HTTP auth, uses password provided after `:` |
| `-o <FILE>` | Write output to file, note this is a lowercase `o` |

## wget
| Option       | Description |
| ------------- | ---------- |
| `--ask-password` | Prompt for HTTP auth password |
| `--user=<USER>` | User for HTTP auth |
| `--password=<PASSWORD>` | Password for HTTP Auth |
| `--post-data=<DATA_URL_ENC>` | Post URL encoded data |
| `--no-check-certificate` | Ignore SSL verification issues |
| `-ca-certificate=<PATH>` | Set cert for TLS verification |
| `-O <FILE>` | Write output to file, note this is a uppercase `o` |