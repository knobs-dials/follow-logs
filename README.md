follow_text_logs
================

Quickly follow (system) logs, without ancient or rotated-compressed stuff. Other than that it's tail -F :)
![follow_text_log](http://i152.photobucket.com/albums/s171/scarfboy/linkto_serious/follow_text_logs.png)


This script...
- looks for logs - primarily system logs (e.g. /var/log)
  and optionally optionally in your homedir  (optional: typically slow and includes nonsense, but sometimes convenient)
- checks whether they were recently written to  (by mtime)
- ignores things that do not look like logs, to ignore things like compressed rotated logs)
  (checks whether a KB or so is text/binary. implementation could be smarter and faster)
- optionally filters which files to show  (substring whitelist on their complete path)
...then follows all the filenames that are left (a `tail` subprocess)


Current experiment: keep looking for logs in the background, every ten seconds.
When the set of matched filenames has *more* files, it re-launches the tail.
This can be useful for things that may create logs later, such as samba.


TODO:
- Needs argument parsing
- Figure out whether there are logs in other places we may wish to look for