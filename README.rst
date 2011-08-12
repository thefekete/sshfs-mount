##################
sshfs-mount README
##################
Mounts sshfs shares
*******************

In order to use this script, just download it, make a copy named something
like 'sshfs-work' and then fill out the variables with your information.

After that you can mount, unmount and remount the share with the following
commands::

    $> sshfs-work mount
    $> sshfs-work unmount
    $> sshfs-work remount

This script uses the package libnotify-bin for notifications which
can be installed with:
    sudo apt-get install libnotify-bin
