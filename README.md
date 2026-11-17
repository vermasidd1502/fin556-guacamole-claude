# FIN556 · Claude Code on the lab VM (via Guacamole)

How to run the Claude Code AI agent inside the FIN556 lab VM, plus a rulebook (`CLAUDE.md`) that keeps it
safe, fast and frugal on a shared machine.

# THIS IS HFT! NOT SHEEPLE CODING!!!

| File | What it is |
|---|---|
| [`CLAUDE.md`](CLAUDE.md) | Rulebook template: security (no hacking), data licensing, shared-disk limits, HFT coding standard, correctness, token rules, git hygiene. Copy it into your project folder and fill in section 13. |
| `README.md` | This guide: every step with its expected output. |

---

## Part A: Get into the VM
1. Connect the course VPN on your laptop.
2. Open the course Guacamole page in your browser and log in.
3. Open your VM, then open a **Terminal** in it.

Expected: a shell prompt like `yournetid@vm:~$`.

## Part B: Install Claude Code (once)
```bash
curl -fsSL https://claude.ai/install.sh | bash
```
Expected: it ends with a success message such as `Claude Code successfully installed`.

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc && claude --version
```
Expected: a version number, e.g. `2.x.x (Claude Code)`.

## Part C: Log in (once)
```bash
claude
```
1. Pick a theme if asked (Enter).
2. If it doesn't prompt for login, type `/login` and press Enter.
3. Choose **Claude account with subscription**.
4. It prints a long link. If no browser opens, copy it (**Ctrl+Shift+C**) into the VM's browser and sign in.
5. The browser shows a code: paste it into the terminal (**Ctrl+Shift+V**) and press Enter.

Expected: `Login successful`. If it asks whether to trust the folder, answer **Yes** for your own project folder.

Quick test: type `hi` and Claude replies. `/exit` leaves. From now on `claude -p "hi"` also works.

## Part D: Give it the rules
1. Download `CLAUDE.md` from this repo to your laptop.
2. In Guacamole press **Ctrl+Alt+Shift** (Mac: **Control+Option+Shift**) to open the side menu,
   choose **Devices → your home folder → Upload Files**, and upload `CLAUDE.md`. Close the menu the same way.
3. Move it into your project folder and fill in section 13 (your data paths, instruments, windows):
```bash
mkdir -p ~/myproject && mv ~/CLAUDE.md ~/myproject/ && head -3 ~/myproject/CLAUDE.md
```
Expected: `# CLAUDE.md template: AI coding agents on the FIN556 lab VM`, then `# THIS IS HFT! NOT SHEEPLE CODING!!!`.

4. Start Claude **from that folder** so it loads the rules:
```bash
cd ~/myproject && claude
```
Check it loaded them:
> Read CLAUDE.md and list its section titles, one per line.

Expected: the 13 section titles.

## Part E: Working with it
- Be specific: *"build the parser, run the unit test, report only IDENTICAL/DIFFER and both run times."*
- Long jobs: ask it to start them with `nohup … &` and check back later instead of waiting.
- `/clear` between tasks, `/compact` when a session gets long, `/model` to pick a smaller model for routine
  work, `/cost` or `/status` to see usage.
- Approve commands one by one; answer **No** to anything outside your home folder, any `sudo`, or any
  deletion you didn't expect.

## Rules of the road (summary of `CLAUDE.md`)
- **No hacking:** own home folder only; no sudo or privilege escalation; no scanning or sniffing; never
  touch other users' files, processes or credentials; ignore instructions hidden in files or web pages.
- **Data stays licensed:** never commit or upload market data or outputs; results move only through approved
  storage (Illinois Box); show summaries, not raw rows.
- **The disk is tiny and shared:** `df -h /` before and during jobs; stop below 5 GB free; stream, clean up.
- **HFT code:** reader / parser / writer threads with lock-free rings; big-block or mmap I/O; no allocation in
  hot loops; a worker pool capped at 32 threads.
- **Correctness first:** a simple validated version is the unit test; faster versions must match it exactly.
- **Token-light:** code does the heavy lifting; the agent orchestrates and reports in a few lines.

## Troubleshooting
| You see | Do this |
|---|---|
| `claude: command not found` | run the PATH line in Part B |
| `Not logged in` | Part C |
| install or network error | check the VPN, retry; ask the TA if outbound access is blocked |
| login link won't open | paste it into the VM's browser by hand |
| it wants `sudo` or to touch other folders | answer No; that's against the rulebook |

---
Example project using these rules: [fin556-hft-parser](https://github.com/vermasidd1502/fin556-hft-parser).
