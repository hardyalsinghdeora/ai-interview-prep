# Linux and Git Commands Cheatsheet

Covers the Linux commands worth knowing for processes, files, networking, logs and debugging, plus Git commands including rebase, reflog, bisect and recovery.

> Related: [Debugging and troubleshooting](../topics/12-debugging-troubleshooting/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md) · [containers-and-kubernetes.md](containers-and-kubernetes.md) · [observability-and-reliability.md](observability-and-reliability.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [networking-and-web.md](networking-and-web.md)

## Linux: processes

| Command | Purpose |
|---|---|
| `ps aux` | Lists all processes with owner, CPU and memory use. |
| `pgrep -a <name>` | Finds process IDs by name and shows their command lines. |
| `top` or `htop` | Shows live CPU and memory use per process. |
| `kill <pid>` | Sends SIGTERM, asking a process to shut down cleanly. |
| `kill -9 <pid>` | Sends SIGKILL, which cannot be caught; use it only as a last resort. |
| `nohup <cmd> &` | Runs a command in the background so it survives logout. |
| `systemctl status <unit>` | Shows a service's state and recent log lines. |
| `systemctl restart <unit>` | Restarts a systemd service. |
| `uptime` | Shows load averages, a quick signal of pressure on the machine. |
| `nice` and `renice` | Set or change a process's scheduling priority. |

## Linux: files and disk

| Command | Purpose |
|---|---|
| `ls -lah` | Lists files with permissions, readable sizes and hidden files. |
| `find . -name '*.log' -mtime +7` | Finds files by name and age. |
| `du -sh *` | Shows the size of each item, useful for finding what fills a disk. |
| `df -h` | Shows free space per filesystem. |
| `df -i` | Shows inode use; a disk can be "full" through inodes alone. |
| `lsof <file>` | Shows which processes hold a file open. |
| `lsof +L1` | Finds deleted files still held open, which explains missing disk space. |
| `chmod 640 <file>` | Sets file permissions for owner, group and others. |
| `chown user:group <file>` | Changes a file's owner and group. |
| `tar -czf out.tgz <dir>` | Creates a compressed archive; use `-xzf` to extract. |
| `rsync -avz <src> <dest>` | Syncs files efficiently, copying only what changed. |
| `ln -s <target> <link>` | Creates a symbolic link. |

## Linux: text and logs

| Command | Purpose |
|---|---|
| `tail -f <file>` | Follows a log file as new lines arrive. |
| `grep -Rin 'text' <dir>` | Searches recursively, ignoring case, showing line numbers. |
| `grep -C 3 'text' <file>` | Shows matches with three lines of context around each. |
| `less +F <file>` | Follows a file, and lets you pause and scroll back. |
| `journalctl -u <unit> -f` | Follows logs for one systemd service. |
| `journalctl --since '1 hour ago'` | Shows system logs from a recent time window. |
| `dmesg -T` | Shows kernel messages, including out-of-memory kills. |
| `awk '{print $1}' <file>` | Extracts and processes columns of text. |
| `sed -n '10,20p' <file>` | Prints a range of lines; `sed` also does find and replace. |
| `sort`, `uniq -c` | Sort and count repeated lines, used together to rank log entries. |
| `wc -l <file>` | Counts lines in a file. |
| `jq '.field' <file>` | Queries and formats JSON (JavaScript Object Notation). |

```bash
# Top client IPs in an access log
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head
```

## Linux: networking

| Command | Purpose |
|---|---|
| `ss -tulpn` | Lists listening ports and owning processes. |
| `ip addr` | Shows network interfaces and their IP addresses. |
| `ip route` | Shows the routing table. |
| `ping <host>` | Checks basic reachability and round-trip time, if ICMP is allowed. |
| `traceroute <host>` | Shows the network path and where delays begin. |
| `dig <name>` | Queries DNS records, including TTL and the answering server. |
| `curl -v <url>` | Shows the full request and response, including TLS details. |
| `curl -w '%{time_total}\n' -o /dev/null -s <url>` | Measures total request time. |
| `nc -zv <host> <port>` | Tests whether a TCP port accepts connections. |
| `tcpdump -i any port 443` | Captures packets for deep network debugging. |
| `openssl s_client -connect <host>:443` | Inspects a server's TLS certificate and handshake. |

## Linux: performance and debugging

| Command | Purpose |
|---|---|
| `free -h` | Shows memory use; "available" matters more than "free". |
| `vmstat 1` | Shows run queue, swapping, I/O and CPU use each second. |
| `iostat -xz 1` | Shows disk utilisation and latency per device. |
| `mpstat -P ALL 1` | Shows CPU use per core, which reveals a single busy core. |
| `pidstat 1` | Shows CPU use per process over time. |
| `strace -p <pid>` | Traces a process's system calls; adds overhead, so use it carefully in production. |
| `perf top` | Shows live CPU hot spots by function. |
| `ulimit -n` | Shows the open file limit, a common cause of "too many open files". |
| `env` | Prints the environment variables of the current shell. |

## Git: everyday

| Command | Purpose |
|---|---|
| `git status` | Shows staged, unstaged and untracked changes. |
| `git switch -c <branch>` | Creates a branch and moves to it. |
| `git add -p` | Stages selected parts of changes, keeping commits focused. |
| `git commit --amend` | Replaces the latest commit; avoid on commits already shared. |
| `git fetch` | Downloads remote changes without changing your working branch. |
| `git pull --rebase` | Fetches, then replays local commits on top, avoiding merge commits. |
| `git push --force-with-lease` | Force-pushes only if the remote has not moved, safer than `--force`. |
| `git stash` / `git stash pop` | Sets aside uncommitted work and restores it later. |
| `git tag -a v1.0 -m 'msg'` | Creates an annotated tag, usually to mark a release. |

## Git: history and investigation

| Command | Purpose |
|---|---|
| `git log --oneline --graph --all` | Shows a compact view of branch history. |
| `git log -p <file>` | Shows a file's changes commit by commit. |
| `git log -S 'text'` | Finds commits that added or removed the given text. |
| `git diff --staged` | Shows what will go into the next commit. |
| `git blame <file>` | Shows the last commit to touch each line. |
| `git show <commit>` | Shows a commit's message and changes. |
| `git bisect start` | Starts a binary search through history for the commit that introduced a bug. |
| `git bisect good` / `git bisect bad` | Marks commits as working or broken, and Git picks the next to test. |
| `git bisect run <test-cmd>` | Automates the search using a script's exit code. |

## Git: rebase and merge

| Command | Purpose |
|---|---|
| `git merge <branch>` | Combines histories, keeping branch structure with a merge commit when needed. |
| `git merge --squash <branch>` | Stages a branch's changes for one new commit, flattening noisy history. |
| `git rebase main` | Replays your commits on top of main, giving linear history with new commit IDs. |
| `git rebase -i HEAD~3` | Interactively reorders, edits, squashes or drops recent commits. |
| `git rebase --continue` | Carries on after resolving a conflict; `--abort` cancels the rebase. |
| `git cherry-pick <commit>` | Applies one commit onto the current branch, often for a hotfix. |

| Merge vs rebase | Merge | Rebase |
|---|---|---|
| History | Preserved, with branch shape | Linear and rewritten |
| Commit IDs | Unchanged | New IDs for replayed commits |
| Shared branches | Safe | Avoid; it disrupts collaborators |

## Git: undo and recovery

| Command | Purpose |
|---|---|
| `git restore <file>` | Discards uncommitted changes in a file. |
| `git restore --staged <file>` | Unstages a file while keeping its changes. |
| `git reset --soft HEAD~1` | Undoes the last commit, keeping changes staged. |
| `git reset --mixed HEAD~1` | Undoes the last commit, keeping changes unstaged (the default). |
| `git reset --hard <commit>` | Moves the branch and discards local changes, so check first. |
| `git revert <commit>` | Adds a new commit undoing an old one; the safe option on shared branches. |
| `git reflog` | Lists where HEAD has been, the main tool for finding "lost" commits. |
| `git branch <name> <commit>` | Creates a branch at a recovered commit, saving it from cleanup. |
| `git fsck --lost-found` | Finds unreachable objects when the reflog does not help. |
| `git clean -fdn` | Previews removal of untracked files; drop the `n` to actually delete. |

```bash
# Recover from a bad reset or rebase
git reflog                      # find the commit from before the mistake
git branch rescue <commit>      # keep it safe on a new branch
```

Reflog is local and its entries eventually expire, so recover soon. A secret pushed to a remote must be rotated, because rewriting history does not remove existing copies.
