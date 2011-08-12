#!/bin/bash

# sshfs-mount - Mounts sshfs shares
# Copyright (C) 2011 - Daniel Fekete <thefekete@gmail.com>
# 
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.


# Usage: sshfs-mount mount|unmount|remount
# 
# This script uses the package libnotify-bin for notifications which
# can be installed with:
#     sudo apt-get install libnotify-bin
# 
# Just fill in the variables with your parameters and have fun!

REMOTE_USER="thedude"  # user name on remote host
REMOTE_HOST="example.com"
REMOTE_PATH="/remote/path"
SSH_PORT=22
MOUNT_POINT="/local/path"
URI="$REMOTE_USER@$REMOTE_HOST:$REMOTE_PATH"

case "$1" in
    mount)
        if ! mount | grep "$URI on $MOUNT_POINT" > /dev/null
        then
            echo "Mounting $URI on $MOUNT_POINT... "
            sshfs -C \
                    -o port=$SSH_PORT \
                    -o idmap=user \
                    -o reconnect \
                    -o follow_symlinks \
                    -o transform_symlinks \
                    $URI $MOUNT_POINT &&
            ( # Success
                echo "Done."
                notify-send -i server 'Successfully Mounted' "$URI" &> /dev/null
                exit 0) ||
            ( # Failure
                echo "FAIL!"
                notify-send -i error 'Failed to mount' "$URI" &> /dev/null
                exit 1
            )
        else
            echo "Remote folder is already mounted"
            echo "Nothing to do, exiting"
            exit 1
        fi
        ;;

    unmount)
        if mount | grep "$URI on $MOUNT_POINT" > /dev/null
        then
            echo "Unmounting $URI from $MOUNT_POINT... "
            fusermount -u $MOUNT_POINT &&
            ( # Success
                echo "Done."
                notify-send -i server 'Successfully Un-Mounted' "$URI" &> /dev/null
                exit 0) ||
            ( # Failure
                echo "FAIL!"
                notify-send -i error 'Failed to unmount' "$URI" &> /dev/null
                exit 1
            )
        else
            echo "Remote folder not mounted yet"
            echo "Nothing to do, exiting"
            exit 1
        fi
        ;;

    remount)
        $0 unmount &&
        $0 mount
        ;;

    *)
        echo "sshfs-mount Copyright (C) 2011 Daniel Fekete"
        echo "This program comes with ABSOLUTELY NO WARRANTY. Please see the"
        echo "source for information on licensing."
        echo
        echo "Usage: $0 mount|unmount|remount"
        exit
        ;;
esac

