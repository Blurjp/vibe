# vibe

A tiny CLI tool for macOS that captures screen regions and helps debug issues using Claude Code CLI.

## Features

- **Region screenshot capture** - Select any area of your screen to debug
- **Context collection** - Automatically includes git status, diffs, and terminal logs
- **Claude Code integration** - Works seamlessly with Claude Code CLI
- **Session tracking** - Maintains a log of debugging sessions
- **Single workflow** - Do everything from within Claude Code CLI

## Requirements

- macOS (uses `screencapture` command)
- Node.js
- Claude Code CLI installed

## Installation

### Quick Install (one command)

```bash
curl -sSL https://raw.githubusercontent.com/Blurjp/vibe/main/install.sh | bash
```

Or clone and run:

```bash
git clone https://github.com/Blurjp/vibe.git
cd vibe
./install.sh
```

### Manual Install

1. Clone this repo
2. Run the init command:

```bash
./tools/vibe/vibe init
```

3. Set up Claude Code integration (creates slash commands):

```bash
cp tools/vibe/claude-commands/*.md ~/.claude/commands/
```

This creates:
- `.vibedbg/` directory for screenshots
- Claude Code commands (`/vibe-select`, `/vibe-ask`)

## Usage

### Recommended: Use within Claude Code CLI

Everything happens in one interface:

```bash
# Start Claude Code CLI
claude

# Then inside Claude Code, use:
/vibe-select          # Capture screen region
/vibe-ask             # Analyze and fix the issue
```

### Alternative: Direct CLI commands

```bash
./tools/vibe/vibe select --note "login error"
./tools/vibe/vibe ask "Fix the error"
```

## Workflow

### 1. Capture the issue

```bash
/vibe-select
```

This opens macOS's screenshot tool. Select the region showing the error/bug.
The screenshot is saved to `.vibedbg/region.png`.

### 2. (Optional) Add logs

If you have backend errors or stack traces:

```bash
pbpaste > .vibedbg/terminal.log
```

### 3. Ask Claude to fix it

```bash
/vibe-ask
```

Claude will:
1. Read the screenshot
2. Check git status and diffs
3. Review terminal logs
4. Search the codebase
5. Propose and implement fixes
6. Verify the changes

## Claude Code Commands

| Command | Description |
|---------|-------------|
| `/vibe-select` | Capture a screen region |
| `/vibe-ask` | Analyze screenshot and fix issues |

## Direct CLI Commands

| Command | Description |
|---------|-------------|
| `vibe init` | Initialize `.vibedbg/` directory |
| `vibe select [--note "text"]` | Capture a screen region |
| `vibe ask "instruction" [options]` | Ask Claude to analyze and fix |

## Options for `vibe ask`

| Option | Description |
|--------|-------------|
| `--model "name"` | Use specific Claude model |
| `--no-diff` | Skip git diff in prompt |
| `--logs <path>` | Use custom log file |
| `--tail <n>` | Number of log lines to include (default: 200) |

## Examples

```bash
# Using Claude Code CLI (recommended)
claude
> /vibe-select
> /vibe-ask

# Using direct CLI
./tools/vibe/vibe select --note "500 error on API call"
./tools/vibe/vibe ask "Fix the API error shown"

# With custom log file
./tools/vibe/vibe ask "Debug this" --logs /tmp/backend.log --tail 500

# Skip git context
./tools/vibe/vibe ask "Just look at the screenshot" --no-diff
```

## Optional: Shell Alias

Add to `~/.zshrc` or `~/.bashrc`:

```bash
alias vibe="$PWD/tools/vibe/vibe"
```

Then use:

```bash
vibe select --note "what this is"
vibe ask "Fix the bug"
```

## Files Created

```
.vibedbg/
├── region.png      # The captured screenshot
├── region.json     # Capture metadata (timestamp, note)
├── session.md      # Session log for tracking debug sessions
└── terminal.log    # Terminal/backend logs (you populate this)

~/.claude/commands/
├── vibe-select.md  # Claude Code: Capture screenshot
└── vibe-ask.md     # Claude Code: Analyze and fix
```

## How It Works

1. **Capture**: `/vibe-select` uses `screencapture -i` to select a region
2. **Collect**: `/vibe-ask` gathers context:
   - Screenshot file path
   - Git branch, status, and diffs
   - Terminal logs (last N lines)
3. **Analyze**: Claude Code reads the screenshot and identifies issues
4. **Fix**: Proposes and implements surgical fixes
5. **Verify**: Runs commands to confirm the fix works

## Debugging Process

When you use `/vibe-ask`, Claude follows this process:

1. **Facts Observed**: Lists visible errors, stack traces, filenames
2. **Hypotheses**: Ranks possible causes
3. **Evidence Gathered**: Searches codebase, opens relevant files
4. **Fix Implemented**: Makes smallest safe change
5. **Verification**: Confirms the fix works

## License

MIT
