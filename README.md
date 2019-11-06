Example 

```
 # follow-logs loca err

 ==> /var/log/apache2/munin.error <==
 [Fri Apr 21 11:28:10.735998 2017] [:error] [pid 24665] [client 66.181.184.185:21300] script '/var/www/wiki-helpful/wp-login.php' not found or unable to stat

 ==> /var/log/apache2/error.log <==
 [Fri Apr 21 12:44:13.315519 2017] [:error] [pid 12392] [client 89.43.107.45:51899] script '/var/www/default/wp-login.php' not found or unable to stat

```

Some niceness around tail -F *
- launches tail on a filtered set of files - and re-launches it if more files match later. (Useful for things that create logs on the fly, such as samba)

- avoids old logs
- avoids compressed logs and binary files (note: without this tool, you might like to do tail -F *[^z])
- optional path substring whitelist. For example `follow-logs error local` will mostly get you apache error logs, and e.g. local0.log

- optionally looks in your homedir (sometimes convenient, often just adds nonsense)



TODO:
- Needs proper argument parsing. Syntax will change.

- see about further default paths to look for logs. Suggestions?

