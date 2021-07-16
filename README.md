"Just show me all relevant logs" in fewer keystrokes.
- Reads from 
  - **files** 
    - by default under under `/log/var` and `/var/lib/log/`
    - avoiding old logs, and compressed logs and other binary files
  - **systemd logs**
- Lets you **filter** filenames and unit names, including/excluding by substrings (in absolute pathname, and unit name)
  - e.g. `-n access` goes a long way to show everything except apache access logs
  - e.g. `-o error,local` goes a long way to show only apache errors and a LOCAL0 log I have
- Picks up new matching logs as they appear
- unified output format
  - coloring by source, as it's easier to parse visually

## EXAMPLE
```  
# follow-logs -n apache
Changing journalctl to listen to 223 units
Changing tail to listen to 74 files
                error.log: [2021-06-26  17:39:36] INFO util/util_conv_string converting metadata from UTF-8 to ISO8859-1
               [influxdb]: [retention] 2021/06/26 16:55:14 retention policy shard deletion check commencing
            [laptop-mode]: Laptop mode
            [laptop-mode]: enabled, not active [unchanged]
            [syslog/CRON]: pam_unix(cron:session): session opened for user root by (uid=0)
            [syslog/CRON]: pam_unix(cron:session): session closed for user root
           munin-node.log: 2021/06/26-17:40:12 [748] Error output from netuser:
           munin-node.log: 2021/06/26-17:40:12 [748]    cat: /dev/shm/munin_netuser_vals_conf: No such file or directory
           munin-node.log: 2021/06/26-17:40:12 [748] Service 'netuser' exited with status 1/0.
```

## Arguments
```
usage: follow-logs [-h] [-o ONLYS] [-n NOTS] [--recency RECENCY]
                   [--check-interval CHECK_INTERVAL] [--scandirs SCANDIRS]
                   [--home] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -o ONLYS, --onlys ONLYS
                        if specified, matches only names with one of these
                        substrings (comma separated)
  -n NOTS, --nots NOTS  if specified, everything except names with one of
                        these substrings (comma separated)
  --recency RECENCY     ignore logs with mtime/ctime older than this, defaults
                        to 1w
  --check-interval CHECK_INTERVAL
                        check for log source changes every this often,
                        defaults to 30s
  --scandirs SCANDIRS   paths to look for logs under, comma-separated,
                        defaults to /var/log/,/var/lib/log/
  --home                look for log-ish text files in homedir, defaults off
                        since it's slow and may turn up crud.
  -C, --no-color        no coloring, even when context seems to support it.
  -v, --verbose         debug verbosity
```

## TODO:
- it's almost certain there are some edge cases in the tail imitation that I haven't thought of yet. I'll get to it.

- systemd related:
  - check that the systemd logic actually picks up new units
  - think of what non-units to show (e.g. .scope for login sessions are nice to see)

- think about optimizations. Scanning a /var/log with thousands of files is slow, hence the low-ish default scan speed 
  - In particular, consider inotify or similar

- 'start with n recent lines when opening logs' in both log sources

- detangle code to ease adding further log sources (e.g. docker logs)

- see about using more colors

## Notes:
- will work without helpers_shellcolor, but coloring the source names is nice for visual parsing

- has its own imitation of `tail -F`, because the tail command didn't seem to deal with logrotate yoinking the file for more than a second or two
