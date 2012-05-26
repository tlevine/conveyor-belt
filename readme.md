Conveyor Belt
=====

Conveyor Belt is a script that makes distributed email archival and querying easy and reliable.

### Archiving email
Conveyor Belt converts emails from maildir format into a series of SQLite files. First, edit the configuration file to specify
* the location of the maildir (`MAILDIR`)
* the location(s) of the resulting SQLite files. (`REFINERIES`)
* whether a new SQLite file should be started each year, month or day (`TRUCKLOAD`)

A simple one `~/.conveyor-belt` looks like this.

    # ~/.conveyor-belt
    
    # Location of the maildir
    MAILDIR=~/mail
    
    # Save files to a local directory.
    REFINERIES=~/conveyor-belt-backup

    # Make a new file for each month
    TRUCKLOAD=month

You can get sort of fancy with the `REFINERIES` variable.
What if you want to send files to another server?

    # If you specify a hostname, files will be sent with scp.
    REFINERIES=tlevine@thomaslevine.com:~/conveyor-belt-backup

What if you want files sent different places depending on the date?

    # If you specify date flags (see `man date`), files will be sent
    # different places depending on the date(s) of the contained emails.
    # For example, this will send SQLite files for May 2012 emails to
    # email-2012-05.thomaslevine.com:~/conveyor-belt-backup
    # Note that the date flags are independent of the TRUCKLOAD variable.
    REFINERIES=tlevine@email-%Y-%m.thomaslevine.com:~/conveyor-belt-backup

    # If you specify multiple refineries as an array, files will go to all the places.
    REFINERIES=( ~/conveyor-belt-backup thomaslevine.com:~/conveyor-belt-backup/%Y )

Second, create 

, one file for each year, day or month
