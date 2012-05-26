Conveyor Belt
=====

Conveyor Belt is a script that makes distributed email archival and querying easy and reliable.

### Archiving email
Conveyor Belt converts emails from maildir format into a series of SQLite files. First, edit the configuration file to specify the locations of the maildir and of the resulting SQLite files. (These locations are called "refineries".) You need to define `MAILDIR` and `REFINERIES`, so a simple one looks like this.

    # ~/.conveyor-belt
    
    # Location of the maildir
    # MAILDIR=~/mail
    
    # Save files to a local directory.
    # REFINERIES=~/conveyor-belt-backup

What if you want to send files to another server?

    # If you specify a hostname, files will be sent with scp.
    # REFINERIES=thomaslevine.com:~/conveyor-belt-backup

    # If you specify date flags (see `man date`), files will be sent
    # different places depending on the 

    # If you specify multiple refineries, files will go to all the places
    # REFINERIES=( ~/conveyor-belt-backup tlevine@thomaslevine.com:~/conveyor-belt-backup )

Second, create 

, one file for each year, day or month
