"Just show me all relevant logs" in fewer keystrokes.
- Reads from 
  - **files** under `/log/var`, avoiding old logs, compressed logs, and binary files
  - **systemd logs**
- Lets you **filter** filenames and unit names, including/excluding by substrings
- Picks up new matching logs as they appear


## EXAMPLE
```  
# follow-logs -apache
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


## TODO:
- add argument parsing. Syntax will change
  - including "ignore non-unit systemd messages"
  - 'start with n recent lines' in both log sources

- check that the systemd logic actually picks up new units

- detangle code to ease adding further log sources


## Notes:
- helpers_shellcolor is not required, but adds colors to source names, which helps visual parsing

