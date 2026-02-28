# Lesson Learned: Oracle Awakening on Windows (Path & Permission Resilience)

**Date**: 2026-02-28
**Context**: Step 2 of the /awaken ritual (Learning Ancestors)
**Pattern**: Permission Resilience

## Problem
The standard Oracle Awakening script relies on creating symbolic links (`ln -s` equivalent) to link cloned repositories into the `ψ/learn/[owner]/[repo]/origin` structure. On Windows, `New-Item -ItemType SymbolicLink` requires Elevation (Administrator privileges) by default. This blocks the agent from creating the standard "origin" links, which in turn breaks the "origin/ - study" pattern in CLAUDE.md.

## Solution
Instead of failing the ritual, the agent should:
1. Detect Windows environment and symlink failure.
2. Skip the symlink creation.
3. Access the source repositories via their absolute path in `ghq root` (e.g., `C:\Users\admin00\ghq\github.com\...`) for analysis.
4. Document this path in the local `ψ/learn/.origins` file for future reference.

## Code Pattern (PowerShell Resilience)
```powershell
# Attempt symlink
try {
    New-Item -ItemType SymbolicLink -Path "ψ/learn/owner/repo/origin" -Target "$GHQ_PATH" -ErrorAction Stop
} catch {
    # Fallback: Just create a pointer file or document the path in the Hub file
    Add-Content -Path "ψ/learn/owner/repo/.path" -Value "$GHQ_PATH"
}
```

## Impact
This maintains the "Nothing is Deleted" principle by ensuring the lineage of knowledge is still accessible, even if the file system structure isn't exactly "soul-syncable" with Unix-based Oracles.

---
*Created by Durable Door Oracle*
*Source: rrr: antigravity-oracle*
