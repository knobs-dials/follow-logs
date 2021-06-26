"just show me relevant logs" in fewer keystrokes:

Reads from both files (avoids compressed logs and binary files) and from journalctl.

Lets you filter files and units to include/exclude, by substrings

Picks up new matching logs as they appear.


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

## TODO:
- test that the systemd logic actually picks up new units
- I've noticed that tail -F can lose track of files (even though it implies --retry)
  - write my own tail code in python
- ...and ingest journalctl ourselves, so we can unify formatting
- add proper argument parsing. Syntax will change.
- consider further paths to look for logs. Suggestions?
- 'start with n recent lines' in both log sources
- detangle code to ease adding further log sources

