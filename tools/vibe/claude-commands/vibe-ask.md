---
description: Analyze the vibe screenshot and fix issues
---

Please read the screenshot at `.vibedbg/region.png` and help me fix the issue shown.

First, check if the screenshot exists and gather context:

```bash
ls -la .vibedbg/
cat .vibedbg/region.json 2>/dev/null
git status --porcelain=v1
git diff --stat 2>/dev/null
tail -n 200 .vibedbg/terminal.log 2>/dev/null
```

Then:
1. Read the screenshot at `.vibedbg/region.png`
2. Identify any errors, bugs, or issues visible in the image
3. Search the codebase for relevant files
4. Propose and implement fixes
5. Verify the changes work

---

**Your task:** Read the screenshot and help fix the issue shown. Follow this process:

1. **Facts Observed**: List visible error messages, status codes, stack traces, filenames, line numbers, URLs
2. **Hypotheses**: Rank 2-4 possible causes and what evidence would confirm
3. **Evidence Gathered**: Search codebase, open relevant files, run minimal commands
4. **Fix Implemented**: Make the smallest safe change. Prefer surgical edits
5. **Verification**: Run commands to confirm the fix works

**Do NOT:**
- Assume the screenshot is from a browser without evidence
- Hallucinate screenshot content - if unreadable, say so
- Do large refactors
- Add new dependencies unless absolutely necessary
