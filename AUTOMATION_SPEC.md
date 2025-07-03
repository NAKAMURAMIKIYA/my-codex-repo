# Edge 4‑Window Automation – Specification for Codex

---

## 0. Goal

Create a **single PowerShell 5.1 script** (`launch_dashboard.ps1`) that, when executed on Windows 10/11, will:

1. Launch **four** Edge (Chromium) windows.

2. Resize/position them to perfectly fill a **1920 × 1080** desktop in a 2×2 grid (each 960 × 540):

   | Position     | X,Y (px) | URL               | Extra Action                        |
   | ------------ | -------- | ----------------- | ----------------------------------- |
   | Top‑Left     | 0,0      | `MOTIONBOARD_URL` | *none*                              |
   | Top‑Right    | 960,0    | `TBW_URL`         | Select **Monitor #1** from dropdown |
   | Bottom‑Left  | 0,540    | `TBW_URL`         | Select **Monitor #2**               |
   | Bottom‑Right | 960,540  | `TBW_URL`         | Select **Monitor #3**               |

3. The dropdown exists in TBW pages and can be reached by: 1 × `{TAB}` → `{SPACE}` to open → optional `{DOWN}` keystrokes → `{ENTER}`.

4. Provide two optional parameters: `-ScreenWidth` / `-ScreenHeight` for non‑FHD screens (script must compute half‑size automatically).

---

## 1. Environment

| Item                | Value                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| OS                  | Windows 10/11 (Japanese)                                                                                                       |
| PowerShell          | **5.1** (no Core)                                                                                                              |
| Browser             | **Microsoft Edge (Chromium)**                                                                                                  |
| Default DPI         | 100 % (but code must work with any)                                                                                            |
| Edge Paths to probe | 1) `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`  2) `C:\Program Files\Microsoft\Edge\Application\msedge.exe` |

---

## 2. URLs

```text
MOTIONBOARD_URL = http://server39:8787/motionboard/main?mbid=fidevfx2uicgja7begdaaabd2dwm4&boardpath=%2FH_10_%E7%94%9F%E7%94%A3%E5%AE%9F%E7%B8%BE%2F0010_%E7%94%9F%E7%A8%74%E3%83%A9%E3%82%A4%E3%83%B3-%E7%9B%AE%E6%A8%99%E5%8F%B0%E6%95%B0-%E5%AE%9F%E7%B8%BE%E5%8F%B0%E6%95%B0%E7%94%BB%E9%9D%A2_20250320

TBW_URL = http://ys5112/TBWPW0330/TBWPW0330_1.aspx?uid=9998901&bu=3
```

*Both contain **\`\`** – PowerShell must treat them as literal strings.*

---

## 3. Functional Requirements

1. **Edge Launch**
   Use `Start-Process` with `--new-window --window-position=X,Y --window-size=W,H`.
2. **Precise placement**
   Call \*\*Win32 API \*\*\`\` (via `Add-Type`) after launch to guarantee final size/position.
3. **Dropdown selection**
   Use `WScript.Shell.SendKeys`. Default wait between activation and key‑send: **800 ms** (parameterize if needed).
4. **URL quoting**
   Use **here‑strings** `@' … '@` starting & ending in column 1 to avoid `&` parse errors.
5. **Error handling**
   Throw descriptive message if Edge not found.

---

## 4. Deliverables

* \`\` – fully self‑contained, ≤ 300 LOC, UTF‑8.
* Inline comments (English) for future maintenance.

---

## 5. Acceptance Checklist

*

---

## 6. Hints for Codex

* Import user32.dll once; reuse for all windows.
* Wrap repeated code in helper functions `Open-Edge`, `Do-Keys`.
* Keep global variables at top for easy tweaking.

---

*End of spec*
