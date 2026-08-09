# Process Management

## What this is
Every running program on Linux is a **process**, and every process has a unique ID number called a **PID**. This module covers viewing, monitoring, and controlling processes — including running tasks in the background so the terminal stays free for other work.

## Commands used

### Viewing processes
```bash
ps aux          # snapshot of all running processes
top              # live, updating view of processes and resource usage (press q to quit)
```
Key columns: `PID` (process ID), `%CPU`/`%MEM` (resource usage), `STAT` (S=sleeping, I=idle, R=running), `COMMAND` (what's running).

### Stopping a process
```bash
kill <PID>       # politely asks the process to stop (lets it shut down cleanly)
kill -9 <PID>    # force-kills the process (no cleanup) — only if the normal kill doesn't work
```

### Interrupting a running command
```
Ctrl + C   # stops whatever's running in the terminal outright
Ctrl + Z   # pauses the current process and sends it to the background (doesn't kill it)
```

### Background & foreground jobs
```bash
command &   # starts a command directly in the background
jobs        # lists background/stopped jobs in the current terminal session
fg          # brings the most recent background/stopped job to the foreground
bg          # resumes a paused job in the background (only useful for tasks that don't need input)
```

## What I tested
1. Ran `ps aux` and `top`, identified real processes and their resource usage from live output.
2. Found a process's PID with `ps aux | grep <name>` and stopped it with `kill <PID>` — confirmed the process received `SIGHUP/SIGTERM` and closed itself.
3. Started `nano`, paused it with `Ctrl+Z`, confirmed it showed as "Stopped" via `jobs`.
4. Tried `bg` on the paused nano — job stayed "Stopped" since `nano` needs keyboard input and can't usefully run in the background.
5. Used `fg` to correctly bring nano back to the foreground, resuming exactly where it left off.

## Note-to-self
`bg` only makes sense for processes that don't need ongoing input (e.g. a long-running script or download). Interactive programs like `nano` will just sit "Stopped" even after `bg` — `fg` is the right tool to resume those.
