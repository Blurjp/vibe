# vibe

A tiny CLI tool for macOS that captures screen regions and asks Claude Code CLI to debug/fix issues using that image plus repository context.

## Features

- **Region screenshot capture** - Select any area of your screen to debug
- **Context collection** - Automatically includes git status, diffs, and terminal logs
- **Claude Code integration** - Passes everything to Claude Code CLI for analysis
- **Session tracking** - Maintains a log of debugging sessions

## Requirements

- macOS (uses `screencapture` command)
- Node.js
- Claude Code CLI installed and on PATH

## Installation

1. Clone this repo
2. Run the init command:

```bash
./tools/vibe/vibe init
```

This creates `.vibedbg/` directory and adds it to `.gitignore`.

## Usage

### 1. Capture a screen region

```bash
./tools/vibe/vibe select --note "login error on submit"
```

This opens macOS's screenshot tool. Select the region showing the error/bug.
The screenshot is saved to `.vibedbg/region.png`.

### 2. (Optional) Add terminal/backend logs

If you have backend errors or stack traces, paste them into the log file:

```bash
pbpaste > .vibedbg/terminal.log
```

### 3. Ask Claude to fix it

```bash
./tools/vibe/vibe ask "Fix the error shown in the screenshot"
```

This generates a prompt containing:
- The screenshot reference
- Git status and diffs
- Recent terminal logs
- Your instruction

Then passes it to Claude Code CLI.

## Commands

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
# Basic debugging
./tools/vibe/vibe select --note "500 error on API call"
./tools/vibe/vibe ask "Fix the API error shown"

# With custom log file
./tools/vibe/vibe ask "Debug this" --logs /tmp/backend.log --tail 500

# Skip git context
./tools/vibe/vibe ask "Just look at the screenshot" --no-diff

# Use specific model
./tools/vibe/vibe ask "Explain this error" --model claude-opus-4-5
```

## Optional: Shell Alias

Add to `~/.zshrc` or `~/.bashrc`:

```bash
alias vibe="$PWD/tools/vibe/vibe"
```

Then use simply:

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
```

## How It Works

1. **Capture**: `vibe select` uses `screencapture -i` to select a region
2. **Collect**: `vibe ask` gathers context:
   - Screenshot file path
   - Git branch, status, and diffs
   - Terminal logs (last N lines)
3. **Prompt**: Builds a structured prompt for Claude Code
4. **Delegate**: Calls `claude` CLI with the prompt
5. **Track**: Updates session log with results

## License

MIT
