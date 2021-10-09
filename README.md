"Just show me all relevant logs" in fewer keystrokes.
- Reads from file logs and journalctl
- allows filtering of which filenames and unit names, to include or exclude, by substrings
- Picks up new matching logs as they appear
- unifies output format, with colors


## EXAMPLE

Positive filter ('only'): `-o error,local` goes a long way to show only apache errors and a LOCAL0 log I have.

Negative filter ('not') : `-n access` goes a long way to show everything except apache access logs:
![colored logs](/screenshots/somelogs.png?raw=true)

Where
  - systemd is indicated with square brackets, files without  
  - coloring by log name, making it easier to parse visually


## Arguments
```
usage: follow-logs [-h] [-o ONLYS] [-n NOTS] [-T] [--recency RECENCY]
                   [--check-interval CHECK_INTERVAL] [--scandirs SCANDIRS]
                   [--home] [-C] [-t] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -o ONLYS, --onlys ONLYS
                        if specified, matches only names with one of these
                        substrings (comma separated)
  -n NOTS, --nots NOTS  if specified, everything except names with one of
                        these substrings (comma separated)
  -T, --no-journalctl-time
                        by default we add the time field to journalctil lines.
                        Specify this to turn that off.
  --recency RECENCY     ignore logs with mtime/ctime older than this, defaults
                        to 1d
  --check-interval CHECK_INTERVAL
                        check for log source changes every this often,
                        defaults to 30s
  --scandirs SCANDIRS   paths to look for logs under, comma-separated,
                        defaults to /var/log/,/var/lib/log/
  --home                look for log-ish text files in homedir, defaults off
                        since it's slow and may turn up crud.
  -C, --no-color        no coloring, even when context seems to support it.
  -t, --true-color      use true-color escapes, for more colors when you know
                        it's supported
  -v, --verbose         debug verbosity
```

## Notes:
- will work without helpers_shellcolor.py, but coloring the source names is nice for visual parsing

- has its own imitation of `tail -F`, because the tail command didn't seem to deal with logrotate yoinking the file for more than a second or two


## TODO:
- test the tail thing for weird edge cases

- think of what systemd non-units to potentially show (e.g. .scope for login sessions may be nice to see)

- think about optimizations. Scanning a /var/log with thousands of files is slow, hence the low-ish default scan interval 
  - In particular, consider inotify or similar

- detangle code in general, and to ease adding further log sources (e.g. docker logs)
