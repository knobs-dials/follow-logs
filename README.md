"Just show me all relevant logs" in fewer keystrokes.
- Reads 
  - from file logs
  - via systemd's journalctl
  - docker logs
- allows filtering of which filenames / unit names / container names to include or exclude, by substrings
  - filtering not yet implemented for docker, it currently follows all
- Picks up new matching logs as they appear
- unifies output format

Also colors logs by source, for ease of skimming


## EXAMPLE

Positive filter ('only'): `-o error,local` goes a long way to show only apache errors and webapp debug I output to LOCAL0 (a syslog facility).

Negative filter ('not') : `-n access` goes a long way to show everything except apache access logs:
![colored logs](/screenshots/somelogs.png?raw=true)

Where
  - systemd is indicated with [square brackets], docker with {curly brackets}, files without  
  - logs are consistently colored, by name, making it easier to skim the output


## Arguments
```
usage: follow-logs [-h] [-o ONLYS] [-n NOTS] [-T] [--recency RECENCY]
                   [--check-interval CHECK_INTERVAL] [--scandirs SCANDIRS]
                   [--home] [-g GREP] [-G GREP_OUT] [-C] [-t] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -o ONLYS, --onlys ONLYS
                        if specified, matches only names with one of these
                        substrings (comma separated)
  -n NOTS, --nots NOTS  if specified, everything except names with one of
                        these substrings (comma separated)
  -T, --no-time         by default we add the time field to journalctl lines.
                        Specify this to turn that off. We also try to remove
                        date-n-time from the start of messages, but no
                        promises
  --recency RECENCY     ignore logs with mtime/ctime older than this, defaults
                        to 1d
  --check-interval CHECK_INTERVAL
                        check for log source changes every this often,
                        defaults to 30s
  --scandirs SCANDIRS   paths to look for logs under, comma-separated,
                        defaults to /var/log/,/var/lib/log/
  --home                look for log-ish text files in homedir, defaults off
                        since it's slow and may turn up crud.
  -g GREP, --grep GREP  only print messages containing substring
  -G GREP_OUT, --grep-out GREP_OUT
                        don't print messages containing substring
  -C, --no-color        no coloring, even when context seems to support it.
  -t, --true-color      use true-color escapes, for more colors when you know
                        it's supported
  -v, --verbose         debug verbosity (0, 1, 2)

```

## Notes:
- will work without helpers_shellcolor.py, but coloring the source names is handy for you to visually distinguish between log sources

- has its own imitation of `tail -F`, because the tail command didn't seem to deal with logrotate yoinking the file for more than a second or two


## TODO:
- test our own file follower for weird edge cases

- think of what systemd non-units to potentially show (e.g. .scope for login sessions may be nice to see)

- think about optimizations. Scanning a /var/log with thousands of files is slow, hence the low-ish default scan interval 
  - In particular, consider inotify or similar

- detangle code in general, and to ease adding further log sources (e.g. docker logs)
