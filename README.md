## EXAMPLE
```
 # follow-logs loca err inf

 ==> /var/log/apache2/munin.error <==
 [Tue Apr 21 11:28:10.735998 2020] [:error] [pid 24665] [client 66.181.184.385:21300] script '/var/www/munin/wp-login.php' not found or unable to stat

 ==> /var/log/apache2/error.log <==
 [Tue Apr 21 12:44:13.315519 2020] [:error] [pid 12392] [client 89.43.107.345:51899] script '/var/www/default/wp-login.php' not found or unable to stat

-- Logs begin at Tue 2020-04-21 12:20:18 CEST. --
Tue Apr 21 12:44:13.315519 2020
Apr 21 12:47:37 myhost influxd[23109]: [httpd] 192.168.1.2 - - [21/Apr/2020:12:47:37 +0200] "POST /write?db=test&precision=ms HTTP/1.1" 204 0 "-" "Python-urllib/2.7" 64db9bd3-9dcc-11ea-ae89-000000000000 2150

```

## SUMMARY:
- watches text files
  - mostly in /var/log
  - optionally in homedir (sometimes convenient, often just adds nonsense)
  - avoids old files logs, by mtime
  - avoids compressed logs and binary files, to avoid a garbled shell
- watches systemd service unit names
- relaunches repective log follower (tail, journalctl) when the set of matches changes

- lets you filter what logs to follow via whitelist and blacklist substrings


## TODO:
- actually test that the systemd logic works

- proper argument parsing. Syntax will change.

- see about further default paths to look for logs. Suggestions?

## CONSIDER:
- imitating tail in code, so we can control output better (e.g. tab-separated with log name as first field)

- detangle code to ease adding further log sources

## SIDE NOTES:
- `tail -F /var/log/*[^z2]` goes a long way towards "avoid garbled shell" when you don't have this tool

