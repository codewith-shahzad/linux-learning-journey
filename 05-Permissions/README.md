# File Permissions

## What this is
Every file and directory in Linux has an owner, a group, and a set of permissions controlling who can read, write, or execute it. Permissions are the foundation for securing a multi-user system.

## The permission string
Running `ls -l` shows something like:
```
-rwxr-x---
```
Broken into four parts:
- **Type**: `-` = regular file, `d` = directory
- **Owner (u)**: `rwx` — what the file's owner can do
- **Group (g)**: `r-x` — what the owning group can do
- **Others (o)**: `---` — what everyone else can do

`r` = read, `w` = write, `x` = execute. A `-` means that permission is not granted.

## Example
A file owned by user `ali`, group `devs`, with permissions `rwxr-x---`:
- `ali` (the owner) has full read/write/execute
- Anyone in the `devs` group can read and execute, but not write
- Anyone outside owner/group (like a user `zara` not in `devs`) has **no access at all**

## Commands used

### Changing permissions — symbolic mode
```bash
chmod o+r filename    # add read for others
chmod g-w filename    # remove write from group
chmod u+x filename    # add execute for owner
```
`u` = user/owner, `g` = group, `o` = others, `+`/`-` = add/remove.

### Changing permissions — numeric (octal) mode
Each permission has a value: `r=4`, `w=2`, `x=1`. Add them up per category.
```bash
chmod 750 filename   # owner=7(rwx), group=5(r-x), others=0(---)
chmod 644 filename   # owner=6(rw-), group=4(r--), others=4(r--)
```

### Applying recursively
```bash
chmod -R 755 directory/   # applies permissions to the directory AND everything inside it
```
Useful when a whole folder tree needs the same permission change, not just one file.

### Changing ownership
```bash
sudo chown ali filename          # change owner to ali
sudo chown ali:devs filename     # change owner to ali AND group to devs
sudo chgrp devs filename         # change only the group
```

## What I tested
1. Interpreted a real permission string (`rwxr-x---`) and correctly identified that a user outside the owning group has no access.
2. Identified the fix (`chmod o+r`) to grant read access to others.
3. Understood `-R` as the recursive flag for applying permission changes across a directory tree.

## Note-to-self
"Others" in Linux permissions is a specific category (`o`), not literally everyone — it means people outside the owner and the owning group. Easy to say "anyone" loosely, but worth being precise about `u/g/o` since interviewers tend to probe this exact distinction.
