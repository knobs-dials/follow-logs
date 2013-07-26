follow_text_logs
================

Convenience around tail.
![follow_text_log](http://i152.photobucket.com/albums/s171/scarfboy/linkto_serious/follow_text_logs.png)



I got tired of typing `tail -F /var/log/*` and all the ancient logs and binary crud that would spit out.
This script...
- looks for system logs (e.g. /var/log)
  and optionally optionally in your homedir  (typically just slow and includes nonsense, but can sometimes be pretty convenient)
- checks whether they were recently written to  (by mtime)
- ignores things that do not look like logs   (primarily just a text/binary check, to ignore things like compressed rotated logs)
  (implementation could be smarter and faster)
- optionally filters which files to show  (substring whitelist on their complete path)
...then follows all the filenames that are left (a `tail` subprocess)


Current experiment: it keeps checking the same places every ten seconds. 
When the set of matched filenames has new files, it re-launches tail.
This can be useful for things that may create logs later, such as samba.


TODO:
- Needs argument parsing
- Figure out where other *nices put their logs and scan that be default