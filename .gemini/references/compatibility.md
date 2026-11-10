# reference — compatibility

Read when checking model parameters or CLI configuration. Not on load.

---

## runtime target

`gemini-4-argon` (GA) and `gemini-3.8-flash` (GA).
- Context window: 1,048,576 tokens
- Output capacity: 65,536 synchronous default; up to 1,048,576 asynchronous long decode (`background: true`)
- Thinking levels: `low`, `medium` (default), `high` (`minimal` and `thinking_budget` are deprecated)

## core configuration notes

1. **Explicit `high` effort tier.**
   `gemini-4-argon` defaults to `medium` thinking effort unless `thinking_level: "high"` is configured (`/effort high` in CLI, `Thinking: High` in App/IDE). Setting `high` ensures full multi-file architectural depth.

2. **Positive architectural specification.**
   Workspace specifications load cleanly when written in standard systems-engineering terminology: positive deliverable definitions, explicit OS/ABI interfaces, and deterministic resource lifecycles (`GEMINI.md` §4.1).

3. **Workspace task files (`prompt.txt`).**
   Placing multi-part build specifications in `prompt.txt` and invoking `read prompt.txt and execute` keeps complex specifications structured and versioned alongside the codebase.

---

## surface matrix & antigravity cli #1228 routing

| surface | file loading | configuration |
|---|---|---|
| Antigravity App (`>= v2.24.0`) | `GEMINI.md` in project root | select `gemini-4-argon`, set `Thinking: High` |
| Antigravity CLI (`agy >= v1.4.3`) | `GEMINI.md` auto-load | run `/model gemini-4-argon` then `/effort high` |
| Antigravity CLI (`agy < v1.4.3`) | `GEMINI.md` auto-load | requires `gemini-api://gemini-4-argon` URI in `settings.json` (#1228) |
| Antigravity IDE Extensions | `GEMINI.md` in project root | VS Code, JetBrains (`2026.2.1+`), Visual Studio, Zed, Xcode |
| AI Studio / API / Vertex | `system_instruction` | `thinking_level: "high"`, `max_output_tokens: 65536` |

**Antigravity CLI (`agy`) URI Scheme (`#1228`).**
In `agy` versions prior to `1.4.3`, `language_server.parseURL` splits custom model URIs strictly on `://`. The installer configures `"model": "gemini-api://gemini-4-argon"` and `"thinkingLevel": "high"` in `~/.gemini/antigravity-cli/settings.json`.