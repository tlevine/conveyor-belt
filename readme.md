Conveyor Belt
=====

Conveyor Belt is a script that makes distributed email archival and querying easy and reliable.

### Archiving email
Conveyor Belt converts emails from maildir format into a series of SQLite files. First, edit the configuration file to specify the locations of the maildir and of the resulting SQLite files. (These locations are called "refineries".)

    # ~/.conveyor-belt
    
    # Location of the maildir
    # MAILDIR=~/mail
    
    # Directory to save files to
    # REFINERIES=~/conveyor-belt-backup

    # If you specify a hostname, files will be sent with scp.
    # REFINERIES=thomaslevine.com:~/conveyor-belt-backup

    # If you specify multiple refineries, files will go to all the places
    # REFINERIES=( ~/conveyor-belt-backup thomaslevine.com:~/conveyor-belt-backup )

Second, create 

, one file for each year, day or month
