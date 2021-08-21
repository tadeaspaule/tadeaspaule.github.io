+++
title = "symlink'd"
description = "Setting up a shared partition for two Linux distros"
date = 2021-08-21
[taxonomies]
tags = ["doing","linux"]
+++

Recently switched from Arch to Alpine but still kept both on my laptop, so I wanted to have a shared partition where all the common stuff would reside (configs, project folders, etc), while keeping the distro specific things to their own partitions. Here is what I did:

1. Create a partition (using `cfdisk`, `gparted`, whatever)
2. Create a filesystem (`mkfs.ext4 dev/nvme0n1p7`, or whatever your partition name is)
3. Create a mount folder for it on both distros & set ownership
    1. `mkdir /mnt/ssd-shared`
    2. `sudo chown -R $USER:$USER /mnt/ssd-shared`
    3. `chmod 700 /mnt/ssd-shared`
4. Add partition to `/etc/fstab`
    ```bash
    # /dev/nvme0n1p7 (ssd-shared)
    UUID=<uuid> /mnt/ssd-shared ext4 defaults,user,exec 0 2
    ```
5. Setup symlinks to home directory
		
    I ended up creating some scripts to make this nice, they're in [my dotfiles repo](https://github.com/tadeaspaule/dotfiles)

