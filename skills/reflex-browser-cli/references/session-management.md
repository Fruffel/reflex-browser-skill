# Session Management

## Core Principles

1. Use one session per automation flow.
2. Reuse the same session while context is stable.
3. Use `new`/`restart` when state becomes ambiguous.
4. End sessions explicitly with `session_kill`.

## CLI Flags: `--session` and `--profile`

- `--session <name>`:
  - Sets a deterministic browser session name for the shell.
  - Use this only when a coding agent needs to reconnect/reuse the exact same session across multiple commands or tool calls.
  - If omitted, the CLI auto-generates a unique session name (for example `session-a1b2c3d4`) to avoid accidental session sharing.

- `--profile <path>`:
  - Sets a persistent browser profile directory (cookies, local storage, other browser state).
  - Use this only when persistent browser state is intentionally required.
  - If omitted, no explicit persistent profile is forced by the CLI.

- Recommended default for normal use:
  - Start with `reflex-browser` and do not pass either flag unless persistence is required.

## Bootstrap and Reuse

- Shell startup auto-sends `start`.
- `open` also auto-starts session when missing.
- Use `session_list` to inspect existing sessions before reuse.

## Lifecycle Commands

- `start`: start if absent
- `new` / `restart`: force clean session
- `session_kill arg1=<session>`: terminate one session

Global stale sessions are cleaned by the agent background cleanup job.

## Failure Recovery

For transport/protocol failures:

1. reconnect shell
2. run `status`
3. run `session_list`
4. either continue with valid session or run `new`

For action failures with stale/changed DOM:

1. re-check `url`/`title`
2. run `summary`
3. rerun `selector_helper`
4. retry action with updated selector

## Hygiene Checklist

- Do not leave long-lived orphan sessions.
- Kill sessions when flow is complete.
- Prefer deterministic session names for multi-step workflows.
- Use `profile` only when persistence is intentionally required.
