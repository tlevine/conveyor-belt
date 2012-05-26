Conveyor Belt
=====

Conveyor Belt is a minimal shell script that makes distributed email archival and querying easy and reliable. More specifically, it moves email from maildir to sqlite files and provides a way of querying many files in parallel.

Because of its simple design, you can run Conveyor Belt on a generic POSIX box (or even a cluster of generic POSIX boxes), and you can expect to be able to retrieve your backup even if you forget that you used Conveyor Belt to create it.

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
`conveyor-belt save` looks through the local directory and finds the two
most recent dates during which emails were backed up. (Maybe this should be the two most recent, at least for daily, in case the backup happens to be run right at the end of a day and some emails are late).
Conveyor belt looks for new emails starting on these dates, and proceeds
until the current date (If an email was sent after today, it is not backed up.) It adds these files to their respective databases and sends them
to the various refineries.

### File organization
The filename of a Conveyor Belt SQLite file indicates the user whose email is being backed up and the date range of the backup.

    <email address>_<date>.sqlite

It matches this regular expression

    .*_[0-9]{4}(-[0-9]{2}(-[0-9]{2})?)?.sqlite

So here are some valid filenames.

    # Emails to and from tlevine@example.com in May 2012
    tlevine@example.com_2012-05.sqlite

    # Emails to and from tlevine@example.org on May 3, 2012
    tlevine@example.org_2012-05-03.sqlite

A refinery is a directory that contains a bunch of Conveyor Belt SQLite files.

### Querying email

Each SQLite database contains a table called conveyor_belt that looks
like this.

<table>
  <thead>
    <tr>
      <th></th><th></th><th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td><td></td><td></td>
    </tr>
    <tr>
      <td></td><td></td><td></td>
    </tr>
    <tr>
      <td></td><td></td><td></td>
    </tr>
  </tbody>
</table>

It has the following schema

    CREATE TABLE conveyor_belts (
      from TEXT NOT NULL,
      to TEXT NOT NULL, 
      cc TEXT NOT NULL, 
      bcc TEXT NOT NULL, 
      subject TEXT NOT NULL,
      ...
    )

and these indices

    CREATE INDEX from ON conveyor_belts(from)
    CREATE INDEX date ON conveyor_belts(date)
    CREATE INDEX subject ON conveyor_belts(subject)
