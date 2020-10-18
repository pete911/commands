# filesystem

## mount and umount
 - `mount` list all filesystems that are currently mounted
 - `mount <external_type> <mount_point>` mount "external type" under the "mount point" path. e.g. `mount /dev/sda4 /users`. `mount_point` has to be directory.
 - `cat /etc/fstab` list of filesystems normally mounted on the system
 - `umount <directory>` detach filesystem. If the filesystem is in use, open files or processes, we need to close them:
   - `fuser -c <mount_point>` prints processes (PID) that hold references to that filesystem (or use `lsof`)
   - `ps up "PID1 PID2 ..."` - check processes returned by `fuser` command
