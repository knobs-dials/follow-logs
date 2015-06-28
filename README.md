![follow_text_log](http://i152.photobucket.com/albums/s171/scarfboy/linkto_serious/follow_text_logs.png)

Quickly follow the logs you want, avoid rotated and compressed stuff, other than that it's tail -F :)



In more detail:
- looks for system logs (e.g. /var/log)
  - optionally also in your homedir  (sometimes convenient, usually just includes nonsense)
- checks whether they were recently written to  (by mtime, to ignore old and rotated logs)
- ignores files that seem binary, to avoid compressed rotated logs and such
- optionally uses a whitelist specified by you (useful when working on specific projects)

...then follows all the filenames that are left (a `tail` subprocess)

- (experiment:) keep checking those criteria in the background.
  When the set of matched filenames has *more* files, it re-launches the tail.
  This can be useful for things that create logs later, such as samba.


TODO:
- Needs argument parsing
  Needs to change how blacklist works, since options will conflict right now:
   sometimes you want a few specific logs
     follow local0 www
   sometimes you want most except for a few verbose things
     follow -apache -postgres -munin

- see about further paths to look for logs. Suggestions?