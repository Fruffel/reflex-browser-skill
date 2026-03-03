# Protocol Reference (Stateless CLI)

## Transport Model

- Invocation: `reflex-browser <command> [args] [flags]`
- Most commands send exactly one backend action.
- Output: exactly one JSON object on stdout.
- Exit code: `0` on success, `1` on failure.
- Prefer explicit sequential invocations over bundled shell scripts so each step is inspectable.

## Command Contract

Global flags:

- `--config <path>`
- `--profile <path>`
- `--timeout <ms>`

Session contract:

- `start`: `--session <id>` optional
- all other action commands: `--session <id>` required

Bootstrap/open options (`start`, `new`, `restart`, `open` only):

- `--width <px>`
- `--height <px>`
- `--headless <true|false>`
- `--open-wait <domcontentloaded|load|networkidle>`

## Backend Payload Mapping

CLI builds one backend payload with:

- `action`
- `arg1`
- `arg2`
- `arg3`
- `session`
- `profile`
- `options` (only for bootstrap/open commands)

`wait`, `visible`, `enabled`, and `selected` optional timeout positional arg maps to command timeout override, not `arg2`.

Smart preflight behavior:

- if `open` receives a relative URL, CLI requests current URL and resolves `arg1` before sending `open`

Connection lifecycle:

- each CLI invocation opens one WebSocket request lifecycle and closes it after completion/failure

## Output Envelope

All command responses follow:

- `ok`
- `action`
- `session` (effective session when known)
- `timingMs`
- `response` (raw backend response)
- `message` (error details)

Response-consumption rule:

- capture full JSON first, then parse/select fields from that captured response object

Shell helper option:

- source `skills/reflex-browser/scripts/capture_json.sh` (from project root) to enforce capture-first parsing in Bash workflows

Use `url` explicitly to verify page context before selector-heavy sequences.

Read command value contract:

- for `text`, `value`, `attribute`, `property`, `tag`, `title`, and `url`, the CLI normalizes extracted output to `response.data.value`

## Screenshot Response

For `screenshot`:

- `response.data.imageBase64`: base64 PNG
- `response.data.mimeType`: `image/png`
- `response.data.byteSize`: byte length
