"Just show me all relevant logs" in fewer keystrokes.
- Reads from 
  - **files** under `/log/var`, avoiding old logs, compressed logs, and binary files
  - **systemd logs**
- Lets you **filter** filenames and unit names, including/excluding by substrings
- Picks up new matching logs as they appear
- unified output format, coloring by source

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
usage: follow-logs [-h] [--recency RECENCY] [--check-interval CHECK_INTERVAL]
                   [--home] [--scandirs SCANDIRS] [-o ONLYS] [-n NOTS] [-v]

optional arguments:
  -h, --help            show this help message and exit
  --recency RECENCY     ignore logs with mtime/ctime older than this
  --check-interval CHECK_INTERVAL
                        check for log source changes every this often
  --home                look for log-ish text files in homedir, defaults off
                        since it's slow and may turn up crud.
  --scandirs SCANDIRS   paths to look for logs under, comma-separated,
                        defaults to /var/log/,/var/lib/log/
  -o ONLYS, --onlys ONLYS
                        if specified, matches only names with one of these
                        substrings (comma separated)
  -n NOTS, --nots NOTS  if specified, everything except names with one of
                        these substrings (comma separated)
  -v, --verbose         debug verbosity
```

## TODO:
- 'start with n recent lines when opening logs' in both log sources

- check that the systemd logic actually picks up new units

- detangle code to ease adding further log sources


## Notes:
- helpers_shellcolor is not required, but adds colors to source names, which helps visual parsing

