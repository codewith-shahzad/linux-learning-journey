# File Systems & Storage

## What this is
Linux doesn't use drive letters (C:\, D:\) like Windows. Instead, every storage device — the main disk, a USB stick, an external drive — gets attached ("mounted") into folders inside one single tree that starts at `/`. This module covers checking disk space, finding what's using it, and understanding how mounting works.

## Commands used

### Checking overall disk space
```bash
df -h
```
Shows total/used/available space per mounted filesystem, in human-readable form (GB/MB). The important line is usually the one mounted on `/` — that's the real main disk.

### Checking what's using space inside a folder
```bash
du -sh *          # size of everything in the current folder
du -sh ~/snap/*   # break down a specific folder further, e.g. by app
```
`df` tells you how much space is left overall; `du` tells you *which* folder or app is actually using it.

### Checking mounted devices
```bash
mount | grep sda
```
Shows what's currently mounted and where — e.g. confirming the main disk (`/dev/sda2`) is mounted at `/`, with filesystem type `ext4`.

### Auto-mount configuration
```bash
cat /etc/fstab
```
This file tells Linux what to automatically mount at boot, so it doesn't have to be done manually every time. Entries typically reference a disk's UUID (a permanent unique ID) rather than a name like `sda2`, since device names can shift if drives are added/removed but the UUID never changes.

## What I tested
1. Ran `df -h` on a real system and correctly identified the main disk (`/dev/sda2` on `/`) versus irrelevant entries like `tmpfs` (temporary RAM-based storage) and `/dev/sr0` (a virtual CD-ROM, always shows 100% used since it can't be added to).
2. Ran `du -sh *` in the home folder and found that `snap` (730M) was the largest space user — traced further into `~/snap/*` and confirmed Firefox's snap was the main contributor.
3. Compared against a normal-sized folder (`Downloads`, 532KB) to see the contrast between heavy and light space usage.
4. Ran `mount | grep sda` to confirm the main disk's mount point and filesystem type.
5. Read `/etc/fstab` and understood the UUID-based root mount entry and the separate swap entry (`/swap.img`, mounted as `swap` rather than to a folder, since it's memory overflow space, not browsable storage).

## Note-to-self
`df` = how much space is left overall. `du` = which specific folder is using it. Use them together when a disk fills up: `df -h` to spot the full partition, then `du -sh` on likely folders to hunt down the cause.
