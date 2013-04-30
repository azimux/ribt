ribt
====

```
        ,~~~~~~~,
        | ribt! |
        `~~~~~~~`
       /
  @..@
 (````)
```

ribt is a very basic backup utility written in Bourne shell.

It depends on a semi-recent version of rsync to do all the heavy lifting.
It aims to have minimal dependencies and has been tested in bash dash and ash.

ribt stands for 4 commands it supports: restore, init, backup, tag


Disclaimer
==========

This is mostly the result of DRYing up some backup scripts I've
accumulated over the years across multiple machines, and an
excuse to spend a little time learning some shell scripting.

This should go without saying, but I would highly recommend using with caution
and test thoroughly.

And feel free to contribute via pull requests!

Commands
========

### init

Run ```ribt init``` in a directory you wish to backup.  This will initialize
a .ribt file and a .ribt_excludes file.  

In the file ```.ribt``` are two bash variables that you need to set.

* backups_server
This is the hostname of the machine you will be backing up to

* backups_toplevel_dir
This is the subdirectory on the backups_server in which to put the backups

There are a couple optional parameters that can be set.  See the script itself
for details

### backup

To perform a backup, run ```ribt backup``` in the directory you wish to backup

If it is currently April 30th, 2013 9:23:22 and the directory you are backing
up is called /foo/bar/baz then the backup will be placed in:

```sh
$backups_server:$backups_toplevel_dir/`hostname -s`/foo_bar_baz/20130430_092322
```

And a symbolic link called ```current``` will point at this directory.

Further backups use hardlinks between files in the most recent backup and the
new backup in order to save space/time.

### tag

Sometimes a directory name like 20130430_092322 doesn't fully communicate the
significance, if any, of the backup.  The tag command is for giving a more
meaningful name to the backup.

Running ```rbit tag 'about_to_upgrade_udev'``` will create a symbolic link
pointing at whatever ```current``` currently points at with the name
```upgrading_udev```

### restore

Running ```rbit restore 20130430_092322``` will restore the contents of the
current directory to that of the backup ```20130430_092322```  This should
obviously be used with caution as it will erase new data since the backup was
taken.

Running ```rbit restore``` is the same as ```rbit restore current```

Testing
=======

The rbit test suite lives in a separate project called rbit-test, which is
also just a sh script.

It will test the rbit that is at ../rbit/rbit relative to itself

Run it with ```rbit-test```

If you see ```GREAT JUSTICE!``` as the last line of output from rbit-test then
everything passed.  Otherwise something has gone wrong.

Legal Mumbo-Jumbo
=================

Licensed under the MIT license, which can be found in MIT_LICENSE.txt

If you make improvements, it would be awesome if you could open a pull request.