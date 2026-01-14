---
description: Capture a screen region using vibe
---

Please run the following command to capture a screen region:

```bash
./tools/vibe/vibe select
```

This will:
1. Open macOS's screen selection tool
2. Save the selected region to `.vibedbg/region.png`
3. Create metadata in `.vibedbg/region.json`

After capturing, you can use `/vibe-ask` to ask me to analyze and fix issues shown in the screenshot.

Optional: Add a note to describe what you captured:
```bash
./tools/vibe/vibe select --note "describe what this screenshot shows"
```
