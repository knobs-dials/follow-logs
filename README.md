One tool to give you all logs, and filter which to see.

```
 # follow-logs loca err

 ==> /var/log/apache2/munin.error <==
 [Fri Apr 21 11:28:10.735998 2017] [:error] [pid 24665] [client 66.181.184.185:21300] script '/var/www/wiki-helpful/wp-login.php' not found or unable to stat

 ==> /var/log/apache2/error.log <==
 [Fri Apr 21 12:44:13.315519 2017] [:error] [pid 12392] [client 89.43.107.45:51899] script '/var/www/default/wp-login.php' not found or unable to stat

```

Some niceness around tail -F * and journalctl

- watches text files
  - avoids old files logs
  - avoids compressed logs and binary files (note: without this tool, you might like to do tail -F *[^z])
- watches systemd service unit names
- relaunches repective log follower (tail, journalctl) when the set of matches changes

- lets you filter what logs to follow (whitelist, blacklist)

- optionally looks in your homedir (sometimes convenient, often just adds nonsense)



TODO:
- proper argument parsing. Syntax will change.

- see about further default paths to look for logs. Suggestions?


CONSIDER:
- writing my own tail so we can control output more (e.g. tab-separated with log name as first field)
