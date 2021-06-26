"Just show me all relevant logs" in fewer keystrokes.
- Reads from both **files** under /log/var (avoids compressed logs, binary files, and old logs) and from **systemd logs**
- Lets you **filter** filenames and unit names, including/excluding by substrings
- Picks up new matching logs as they appear


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
- write my own tail code in python, because the call to an external `tail -F` loses track of files (even though it implies --retry)
- ...and ingest journalctl ourselves, to unify formatting

- add proper argument parsing. Syntax will change
- test that the systemd logic actually picks up new units
- 'start with n recent lines' in both log sources

- figure out how to stop journalctl reporting from non-units
- detangle code to ease adding further log sources


## Notes:
- helpers_shellcolor is not required, but adds colors to source names, which helps visual parsing

