# Final Exam — Answer Key: System Administration & DevOps Introduction

**Covers:** Part III (Chapters 8-11) and Part IV (Chapters 12-13)
**Total Points:** 20 | **Passing Score:** 12/20 (60%)

---

## Part 1: Multiple Choice (15 × 1 pt = 15 pts)

| # | Answer |
|---|--------|
| 1 | B — dnf install |
| 2 | C — Refreshes the list of available packages |
| 3 | C — apt purge |
| 4 | B — top |
| 5 | B — Sends SIGKILL (force stop) |
| 6 | B — systemctl enable nginx |
| 7 | B — Tells the system to use the Bash interpreter |
| 8 | B — The first argument passed to the script |
| 9 | A — chmod 755 script.sh |
| 10 | A — ip addr |
| 11 | B — 22 |
| 12 | B — git add → git commit → git push |
| 13 | C — git checkout -b new-feature |
| 14 | B — Containers share the host's kernel and are more lightweight |
| 15 | C — CMD |

## Part 2: Short Answer (5 pts)

**16.** (1 pt) `systemctl start` vs `systemctl enable`:
- `systemctl start` starts a service **right now** (does not persist across reboots)
- `systemctl enable` configures a service to start **automatically at boot** (does not start it immediately)

**17.** (2 pts) Merge conflict:
A merge conflict occurs when two branches modified the same lines differently and Git cannot decide which version to keep. To resolve: edit the file to keep the correct version, remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`), then `git add` and `git commit`.

**18.** (2 pts) Three commands for Fedora:
```bash
sudo dnf install htop
systemctl status nginx
journalctl -u nginx --since "1 hour ago"
```
(Also acceptable: `journalctl -u nginx -n 50`)
