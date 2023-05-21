# noita-backup-manager
A command line utility written in Python for managing backups of Noita save archives.

## Setup
The first time you run the tool, it'll run through a short setup process. It'll prompt you for the path to Noita's
save data and for the directory that you want to save backups to. It'll save these settings to a config file
`noitabm.ini` that will sit right next to the `noitabm` script.

## Usage
```
usage: noitabm [-h] [--version] {list,backup,restore,modify} ...

positional arguments:
  {list,backup,restore,modify}
    list                list available session IDs or backup IDs
    backup              create a new backup from the specified slot
    restore             restore the specified backup to the specified slot
    modify              modify an existing backup in-place

options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
```

## List Usage
```
usage: noitabm list [-h] [-c | -a | session_id]

positional arguments:
  session_id    list backup IDs for the specified session ID

options:
  -h, --help    show this help message and exit
  -c, --config  list the values in noitabm.ini
  -a, --all     list all backups for all sessions
```

### Examples:
```
$ noitabm list
Found backups for 2 session(s):

    SESSION ID
    ----------
    20230518-231856
    20230521-033728
```
```
$ noitabm list 20230518-231856
Found 4 backups for session 20230518-231856:

    BACKUP ID          SHORT HASH    DESCRIPTION
    --------------------------------------------
    20230520-174107    a88d24d
    20230521-023818    a01bb65
    20230521-023833    2aef84c       testing some more
    20230521-034344    b09c584       Snapshot of Slot 0
```