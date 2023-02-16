## How-to mergefs

Go there:

https://github.com/trapexit/mergerfs/releases

Download an arm64 deb file, like this

```bash
wget https://github.com/trapexit/mergerfs/releases/download/2.34.1/mergerfs_2.34.1.ubuntu-trusty_arm64.deb
sudo dpkg -i mergerfs_2.34.1.ubuntu-trusty_arm64.deb
sudo apt install -f
```

aaand that's it...

## Fstab examples

Create `/storage`, and run `chown $USER:$USER -R /storage`

Create `disk00X` directory in `/mnt` dir, then mount one of your external disk to it.

Get uuid:

`ls -al /dev/disk/by-uuid/`

Example:
```bash
$ cat /etc/fstab
# UNCONFIGURED FSTAB FOR BASE SYSTEM
/dev/mmcblk0p1    /boot vfat defaults 0 0
/mnt/512MB.swap    none    swap    sw    0    0

UUID=a60d725e-cd1b-40f3-8f23-d087f81f198a /mnt/disk001  ext4    auto,nofail,noatime,rw,user    0   0

/mnt/disk* /storage fuse.mergerfs threads=12,allow_other,use_ino,cache.files=off,dropcacheonclose=true,category.create=mfs,moveonenospc=true,minfreespace=10G,fsname=mergerfsPool,nonempty 0 0
```

In the example above, ext_disk will be mounted in /mnt/diskXXX

All /mnt/disk* will be mounted as mergefs filesystem on /storage

## Reading materials
Fancy & Usefull documentation for mergefs:

https://perfectmediaserver.com/tech-stack/mergerfs/

New users common problem/caveat

https://github.com/trapexit/mergerfs#why-are-all-my-files-ending-up-on-1-drive

And more about policies:

https://github.com/trapexit/mergerfs#policy-descriptions