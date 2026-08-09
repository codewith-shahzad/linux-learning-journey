# Users & Groups

## What this is
Every Linux system needs a way to manage *who* can log in and *what they can access*. That's the job of users and groups.

- **A user** = one individual account with its own login and home directory.
- **A group** = a way to bundle users together so you can grant access to several people at once, instead of one by one.

This ties directly into permissions (see `05-Permissions`): a file's group ownership decides which group gets the group-level `rwx` bits.

## Commands used

### Creating a user
```bash
sudo adduser testuser      # interactive, friendlier version
sudo useradd testuser      # manual, non-interactive version
sudo passwd testuser       # set/change the user's password
```

### Creating a group
```bash
sudo groupadd learner
```

### Adding / removing a user from a group
```bash
sudo usermod -aG learner testuser   # add testuser to the learner group (-aG = append, don't overwrite existing groups)
sudo gpasswd -d testuser learner    # remove testuser from the learner group
```

### Checking user & group info
```bash
cat /etc/passwd | grep testuser   # shows the user's account entry
groups testuser                   # lists which groups a user belongs to
id testuser                       # shows UID, GID, and all group memberships in one line
```

### Deleting a user
```bash
sudo userdel -r testuser   # -r also removes their home directory
```

## What I tested
1. Created `testuser` with `adduser`, set a password.
2. Created a group called `learner`.
3. Added `testuser` to the `learner` group with `usermod -aG`.
4. Verified membership with `groups` and `id`.
5. Removed `testuser` from `learner` with `gpasswd -d`, verified with `groups`.
6. Deleted `testuser` entirely with `userdel -r`, confirmed removal with `id` (correctly returned "no such user").

## Note-to-self
`/etc/passwd` needs the leading `/` — `cat/etc/passwd` and `cat etc/passwd` both fail with "No such file or directory." Small typo, easy to make, worth double-checking.
