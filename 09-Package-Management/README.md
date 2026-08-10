# Package Management

## What this is
Software on Ubuntu is distributed as pre-built **packages** rather than manually compiled from source. `apt` (Advanced Package Tool) installs, updates, and removes these packages, automatically handling dependencies — other software a package needs to run. Packages come from **repositories**, remote servers storing thousands of ready-to-install packages.

## Commands used

### Refreshing the package list
```bash
sudo apt update
```
Downloads the latest catalog of available packages and their versions from the repositories. Does **not** install or change anything — just refreshes what apt knows is available, so later commands act on current information.

### Installing available updates
```bash
sudo apt upgrade
```
Installs newer versions of packages already on the system, based on the catalog `update` just fetched. Must run `update` first, or `upgrade` works off a stale list and may miss real updates.

### Installing new software
```bash
sudo apt install <package-name>
```
Installs new software and automatically resolves and installs any dependencies it needs.

### Removing software
```bash
sudo apt remove <package-name>    # removes the software, keeps config files
sudo apt purge <package-name>     # removes the software AND its config files completely
sudo apt autoremove               # cleans up leftover dependency packages no longer needed
```

### Checking background services
```bash
systemctl status <service-name>
```
Installing software only puts it on disk — it doesn't mean it's running. Some software runs continuously in the background as a **service** (e.g. a web server, SSH). `systemctl` checks/controls whether these services are actively running — a separate job from `apt`, which only handles getting software onto the disk.
```bash
sudo systemctl start <service-name>     # start a service
sudo systemctl stop <service-name>      # stop a service
sudo systemctl enable <service-name>    # auto-start it on boot
```

## What I tested
1. Ran `sudo apt update` on a real system — refreshed the package catalog, saw 134 packages were upgradable.
2. Ran `apt list --upgradable` to preview what would be upgraded.
3. Ran `sudo apt upgrade` — installed all available updates, followed by `sudo apt autoremove` to clean up leftover dependencies.
4. Installed a test package (`neofetch`) with `apt install`, verified it worked by running it.
5. Checked a real background service with `systemctl status ssh`.
6. Removed the test package with `apt remove` to clean up.

## Note-to-self
`apt` = getting software onto the disk. `systemctl` = controlling whether installed background software is actively running. Two separate jobs, often used together — install a service with `apt`, then manage it with `systemctl`.
