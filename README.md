follow_text_logs
================

A wrapper around tail -F with some convenient behaviour:

Looks in a few places for interesting logs.
Saves some typing.

Ignores binary stuff.
Useful to avoid compressed logs garbling your terminal.

Keeps checking the same places. When the set of matched filenames changes,
it re-launches tail -F
This can be useful for things that may create logs later, such as samba.  


Screenshot: 
