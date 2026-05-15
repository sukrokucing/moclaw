# HEARTBEAT.md

## 🐛 BugByte — Roblox Tester Auto-Trigger

Check for new commits in `sukrokucing/roblox-startup` since last check.

### Steps:
1. Run:
   ```bash
   cd /tmp/roblox-startup && git fetch origin && git log origin/main --oneline --since="$(cat /tmp/bugbyte-last-check 2>/dev/null || echo '1970-01-01')" 2>/dev/null
   ```
2. If new commits found:
   - Read all markdown docs: `harvest-rng/docs/GDD.md`, `TECHNICAL_SPEC.md`, `PLAY_GUIDE.md`, `harvest-rng/README.md`
   - Read each changed `.lua` file (`git diff origin/main HEAD~1 --name-only`)
   - Apply BugByte review protocol from `projects/roblox-tester-agent/AGENTS.md`
   - Write report to `harvest-rng/docs/test-reports/YYYY-MM-DD-HH-MM-report.md`
   - Commit + push report
   - Update timestamp: `date -u +%Y-%m-%dT%H:%M:%SZ > /tmp/bugbyte-last-check`
3. If no new commits → `HEARTBEAT_OK`

### State file: `/tmp/bugbyte-last-check`
