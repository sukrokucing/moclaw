# HEARTBEAT.md

## 🐛 BugByte — Roblox Tester Auto-Trigger

Check for new commits in `sukrokucing/roblox-startup` since last check.

### Steps:
1. Ensure repo clone exists:
   ```bash
   if [ ! -d /tmp/roblox-startup/.git ]; then
     # PAT is stored in ~/.git-credentials — no token needed inline
     git clone https://github.com/sukrokucing/roblox-startup /tmp/roblox-startup
   fi
   ```
2. Fetch and check for new commits:
   ```bash
   cd /tmp/roblox-startup && git fetch origin 2>/dev/null
   LAST=$(cat /tmp/bugbyte-last-check 2>/dev/null || echo '1970-01-01')
   NEW=$(git log origin/main --oneline --since="$LAST" 2>/dev/null)
   ```
3. If new commits found:
   - Read all markdown docs: `harvest-rng/docs/GDD.md`, `harvest-rng/docs/TECHNICAL_SPEC.md`, `harvest-rng/docs/PLAY_GUIDE.md`, `harvest-rng/README.md`
   - Pull latest: `git pull origin main`
   - Read each changed `.lua` file (`git diff HEAD~1 --name-only | grep .lua`)
   - Check open GitHub issues first — don't create duplicates
   - Apply BugByte review protocol from `projects/roblox-tester-agent/AGENTS.md`
   - Write report to `harvest-rng/docs/test-reports/YYYY-MM-DD-HH-MM-report.md`
   - Commit + push report
   - File GitHub issues for all 🔴 Blockers and 🟡 Majors with label `roboluau` (Step 5 in AGENTS.md)
   - Close any GitHub issues whose blocker is now fixed (Step 6 in AGENTS.md)
   - Update timestamp: `date -u +%Y-%m-%dT%H:%M:%SZ > /tmp/bugbyte-last-check`
4. If no new commits → check RoboLuau queue (Step below)
5. → `HEARTBEAT_OK`

---

## 🎮 RoboLuau — Auto-Fix Trigger

After BugByte check (or independently), check for open GitHub issues labeled `roboluau`:

```python
import urllib.request, json

TOKEN = open("/home/user/.git-credentials").read().split("@")[0].split("//")[-1]
headers = {"Authorization": f"token {TOKEN}", "User-Agent": "moclaw-heartbeat"}
req = urllib.request.Request(
    "https://api.github.com/repos/sukrokucing/roblox-startup/issues?state=open&labels=roboluau&per_page=10",
    headers=headers
)
with urllib.request.urlopen(req) as r:
    issues = json.loads(r.read())
print(f"RoboLuau queue: {len(issues)} issues")
for i in issues:
    print(f"  #{i['number']} {i['title']}")
```

If issues found → **invoke RoboLuau via Pi** to fix them:

```bash
export PATH="$HOME/.npm-global/bin:$PATH"
export ANTHROPIC_API_KEY=$(cat ~/.anthropic_key 2>/dev/null || echo "")
cd /home/user/.workspace/projects/roblox-startup

# For each open roboluau-labeled issue:
pi -p "Fix GitHub issue #<N>: <title>. Read the issue body for details. Apply the fix described. Commit with message 'fix: <short desc> (closes #<N>)'. Push to main."
```

After RoboLuau fixes and pushes → BugByte CI will re-run automatically and close the issue if checks pass.

### State file: `/tmp/bugbyte-last-check`
### Repo clone: `/tmp/roblox-startup`
### PAT: stored in `~/.git-credentials` (already configured)
