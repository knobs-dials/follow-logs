follow_text_logs
================

![follow_text_log](http://i152.photobucket.com/albums/s171/scarfboy/linkto_serious/follow_text_logs.png)

This script...
- looks for system logs (e.g. /var/log)
  and optionally optionally in your homedir  (optional: often includes nonsense, but sometimes convenient)
- checks whether they were recently written to  (by mtime, to ignore old/rotated logs)
- ignores things that do not look like logs (binary content, to ignore compressed logs)
- optionally uses a whitelist specified by you (useful when working on specific projects)
...then follows all the filenames that are left (a `tail` subprocess)

In other words: quickly follow the logs you want, avoid rotated and compressed stuff, other than that it's tail -F :)



Current experiment: keep checking those criteria in the background.
When the set of matched filenames has *more* files, it re-launches the tail.
This can be useful for things that may create logs later, such as samba.


TODO:
- Needs argument parsing
- see about further paths to look for logs. Suggestions?