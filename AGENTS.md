_This space is yours. Append your own notes below to make it your own._

---

## Lessons Learned — 2026-05-15

### 🔑 Saving secrets / API keys
**Never use `echo "..."` in bash to write API keys to files.** Shell escaping silently corrupts characters (seen: `l` → `V` at position 321 of an OC API key). Always use Python:
```python
with open("/path/to/secret", "w") as f:
    f.write(raw_value)
```
After saving, always verify with a live API call before proceeding.

### ⏱️ Roblox OC API key expiry
The OC API key contains an **embedded JWT that expires 1 hour after copy**. On every session start, check `exp` before using it:
```python
import base64, json, time
raw = open("secrets/oc_api_key").read().strip()
jwt = base64.b64decode(raw)[36:].decode()
payload = json.loads(base64.b64decode(jwt.split('.')[1] + '=='))
print("expired" if time.time() > payload['exp'] else "valid")
```
If expired → ask user for a fresh key immediately.

### 🐚 Shell heredoc with Python causes output interleaving
`python3 << 'EOF' ... EOF` in bash_execute produces garbled output where the heredoc content gets mixed into the output stream. Use a temp file instead:
```python
# Write script to file, then execute
open("/tmp/script.py", "w").write(code)
# then: python3 /tmp/script.py
```
Or use `python3 /dev/stdin << 'PYEOF'` with a unique delimiter.

### 🔒 GitHub auto-close via commit message is unreliable for direct pushes
`closes #N` in a commit message pushed directly to `main` does **not** reliably auto-close issues. GitHub only guarantees auto-close when the commit is merged via a PR. **Always check open issues after a fix push and close manually if needed.**

### 📋 OC Luau Execution — logs vs output
Task state `COMPLETE` doesn't mean logs are in `result["output"]`. The actual print output is in the `/logs` endpoint:
```
GET /cloud/v2/{task_path}/logs
→ response["luauExecutionSessionTaskLogs"][*]["messages"]
```
The task result body `output` field is often a string `"SIM_OK"` (the return value), not the print log.

### 🛠️ BugByte — Phase 2 scope only needs `:write`
The `universe.place.luau-execution-session:write` scope includes the GET (read) endpoints too. You only need `:write` on the API key — `:read` is redundant. But grant both for clarity.
