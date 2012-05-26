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

    # If you specify multiple refineries as an array, files will go to the places.
    REFINERIES=( ~/conveyor-belt-backup /mnt/external-hd/conveyor-belt-backup/ )

You must always specify a local directory, but you may also specify that
files should be sent to another server.

    # If you specify a hostname, files will be sent with scp.
    REFINERIES=( ~/conveyor-belt-backup thomaslevine.com:~/conveyor-belt-backup/%Y )

And what if you want files sent different places depending on the date?

    # If you specify date flags (see `man date`), files will be sent
    # different places depending on the date(s) of the contained emails.
    # For example, this will send SQLite files for May 2012 emails to
    # email-2012-05.thomaslevine.com:~/conveyor-belt-backup
    # Note that the date flags are independent of the TRUCKLOAD variable.
    REFINERIES=( ~/conveyor-belt-backup tlevine@email-%Y-%m.thomaslevine.com:~/conveyor-belt-backup )

Create the various `REFINERIES` directies if they don't exist.

Once that's all configured, run `conveyor-belt save` to perform a backup.
`conveyor-belt save` retrieves the most recent database (maybe this should be the two most recent, at least for daily, in case the backup happens to be run right at the end of a day and some emails are late)

### File organization
