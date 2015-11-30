![follow_text_log](http://i152.photobucket.com/albums/s171/scarfboy/linkto_serious/follow_text_logs.png)

Quickly follow the logs you want, avoid rotated and compressed stuff.

Beyond that it's just tail -F



In more detail:
- looks for system logs (e.g. /var/log)
  - optionally also in your homedir  (sometimes convenient, usually just adds nonsense)
- checks whether file was recently written to, to ignore old and rotated logs
- ignores files that seem binary, to avoid compressed rotated logs and such
- optionally uses a whitelist specified by you (useful when working on specific projects)
...then follows all the filenames that are left.

- (current experiment:) keep checking those criteria in the background.
  When the set of matched filenames has *more* files, it re-launches the tail.
  Can be useful for things that create logs on the fly, such as samba.


TODO:
- Needs argument parsing. 
  sometimes you want most except for a few verbose things
     follow -apache -postgres -munin
  ...but syntax will change. 

- see about further paths to look for logs. Suggestions?