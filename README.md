# noita-backup-manager
A command line utility written in Python for managing backups of Noita save archives.

## Setup
The first time you run the tool, it'll run through a short setup process. 
1. Enter the path to `Nolla_Games_Noita`. The tool will attempt to find it automatically and use that as the default.
2. Enter the directory for storing your backups. The default will be `Nolla_Games_Noita/Backups/`.

The tool will save these two values to `noitabm.ini`, right next to `noitabm`.

## Supported Environments
* Requires Python 3.7+.
* Windows, WSL on Windows, Linux, and Mac should all work.
    * The tool should find `Nolla_Games_Noita` automatically in Windows and WSL.
    * It might not find `Nolla_Games_Noita` automatically in Linux.
    * It definitely won't find `Nolla_Games_Noita` automatically in Mac.
* No additional Python libraries required.

## Usage
### WSL, Linux, and Mac
Make sure `python3` points to Python 3.7+. Then make sure `noitabm` is executable and
in your PATH variable. Then you can run the tool by calling `noitabm` or `python3 noitabm` in a shell.

### Windows
Make sure Python 3.7+ is in your PATH variable. Then you can run the tool by calling `python noitabm`
in a shell.

### Commands
* **config:** set noitabm.ini values from the command line
  * Can print the current contents and location of `noitabm.ini`
  * Can alter `noita_data_path`
  * Can alter `backup_data_path`
* **list:** list available session IDs or backup IDs
  * Defaults to listing all sessions and backups
  * Can list N most recent sessions or N most recent backups
  * Can list all backups for a given session
* **backup:** create a new backup from the specified slot
  * *I strongly recommend Saving and Quitting Noita before creating backups. Weird things may happen otherwise.*
  * Defaults to backing up `save00`
  * Can back up any of the seven built-in slots
* **restore:** restore the specified backup to the specified slot
  * *Again, Save and Quit before restoring a backup.*
  * Can select a backup by (session_id, backup_id)
  * Can select a backup by (session_id, short_hash)
  * Can select a backup by (session_id, long_hash)
  * Defaults to restoring to `save00`
  * Can restore to any of the seven built-in slots
  * Can specify a description for the backup (need to wrap in quotes)
* **modify:** modify an existing backup in-place
  * Can select a backup by (session_id, backup_id)
  * Can select a backup by (session_id, short_hash)
  * Can select a backup by (session_id, long_hash)
  * Can modify the description of a selected backup
  * Can recompute the hash of a selected backup
  * Can recompute the hashes for all backups
* **delete:** delete archived sessions and backups
  * Can delete all backups for a specified session_id
  * Can select an individual backup to delete by (session_id, backup_id)
  * Can select an individual backup to delete by (session_id, short_hash)
  * Can select an individual backup to delete by (session_id, long_hash)

### `>>> noitabm -h`
```
usage: noitabm [-h] [--version] {config,list,backup,restore,modify,delete} ...

positional arguments:
  {config,list,backup,restore,modify,delete}
    config              set noitabm.ini values from the command line
    list                list available session IDs or backup IDs
    backup              create a new backup from the specified slot
    restore             restore the specified backup to the specified slot
    modify              modify an existing backup in-place
    delete              delete archived sessions and backups

options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
```

### `>>> noitabm config -h`
```
usage: noitabm config [-h] {print,edit} ...

positional arguments:
  {print,edit}
    print       print the contents and location of noitabm.ini
    edit        edit config values

options:
  -h, --help    show this help message and exit
```

### `>>> noitabm config edit -h`
```
usage: noitabm config edit [-h] [--noita-data-path NOITA_DATA_PATH] [--backup-data-path BACKUP_DATA_PATH]

options:
  -h, --help            show this help message and exit

modifiables:
  --noita-data-path NOITA_DATA_PATH
                        set the path to Noita's save data (i.e. Nolla_Games_Noita)
  --backup-data-path BACKUP_DATA_PATH
                        set your desired backup storage location
```

### `>>> noitabm list -h`
```
usage: noitabm list [-h] [-b] [-n N] {session} ...

positional arguments:
  {session}
    session   display backups for a specific session ID

options:
  -h, --help  show this help message and exit
  -b          when paired with -n, swaps to last N backups
  -n N        show only the last N sessions
```

### `>>> noitabm list session -h`
```
usage: noitabm list session [-h] session_id

positional arguments:
  session_id

options:
  -h, --help  show this help message and exit
```

### `>>> noitabm backup -h`
```
usage: noitabm backup [-h] [-d DESCRIPTION] [{0,1,2,3,4,5,6}]

positional arguments:
  {0,1,2,3,4,5,6}       the save slot to backup (default: 0)

options:
  -h, --help            show this help message and exit
  -d DESCRIPTION, --description DESCRIPTION
                        Add a description to the backup, so you can easily identify it later
```

### `>>> noitabm restore -h`
```
usage: noitabm restore [-h] (-b BACKUP_ID | -s SHORT_HASH | -l LONG_HASH) [--skip-restore-point] session_id [{0,1,2,3,4,5,6}]

positional arguments:
  session_id            the session ID to restore
  {0,1,2,3,4,5,6}       the save slot to restore to (default: 0)

options:
  -h, --help            show this help message and exit
  -b BACKUP_ID, --backup_id BACKUP_ID
                        the backup ID to restore
  -s SHORT_HASH, --short-hash SHORT_HASH
                        the short-hash to restore
  -l LONG_HASH, --long-hash LONG_HASH
                        the long-hash to restore
  --skip-restore-point  apply the backup without creating a restore point. CURRENT DATA WILL BE PERMANENTLY LOST.
```

### `>>> noitabm modify all -h`
```
usage: noitabm modify all [-h] [--hash-all]

options:
  -h, --help  show this help message and exit
  --hash-all  recompute all hashes
```

### `>>> noitabm modify session -h`
```
usage: noitabm modify session [-h] (-b BACKUP_ID | -s SHORT_HASH | -l LONG_HASH) [--hash] [-d DESCRIPTION] session_id

positional arguments:
  session_id            the session ID to modify

options:
  -h, --help            show this help message and exit
  -b BACKUP_ID, --backup_id BACKUP_ID
                        the backup ID to modify
  -s SHORT_HASH, --short-hash SHORT_HASH
                        the short-hash to modify
  -l LONG_HASH, --long-hash LONG_HASH
                        the long-hash to modify
  --hash                recompute the hash for the backup, in case you've modified the contents
  -d DESCRIPTION, --description DESCRIPTION
                        set a new description for the backup, so you can easily identify it later
```

### `>>> noitabm delete session -h`
```
usage: noitabm delete session [-h] session_id

positional arguments:
  session_id  the session ID to delete

options:
  -h, --help  show this help message and exit
```

### `>>> noitabm delete backup -h`
```
usage: noitabm delete backup [-h] (-b BACKUP_ID | -s SHORT_HASH | -l LONG_HASH) session_id

options:
  -h, --help            show this help message and exit

  session_id            the session ID to modify
  -b BACKUP_ID, --backup_id BACKUP_ID
                        the backup ID to modify
  -s SHORT_HASH, --short-hash SHORT_HASH
                        the short-hash to modify
  -l LONG_HASH, --long-hash LONG_HASH
                        the long-hash to modify
```