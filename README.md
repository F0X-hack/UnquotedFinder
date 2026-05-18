<div align="center">

<img width="700" height="394" alt="skull" src="https://github.com/user-attachments/assets/db31d87d-b782-4aa8-942d-46bc4282ec39" />

# UnquotedFinder

**Windows Unquoted Service Path Vulnerability Hunter**

<img src="https://github.com/user-attachments/assets/f729f31f-a64f-40eb-b6aa-da276a4206ee" width="544" alt="preview" />

*Detect, analyze and report unquoted service path vulnerabilities on Windows — straight from your terminal.*

</div>

---

## About

**UnquotedFinder** is a Windows security tool that scans the registry for services whose binary paths contain spaces but are not enclosed in quotes. This class of misconfiguration can allow a local attacker to escalate privileges to `SYSTEM` by placing a crafted executable in an intercepted path segment.

The tool scans all installed services, evaluates each exploit vector, checks directory permissions, and classifies findings by risk level — with a color-coded terminal UI inspired by classic hacking aesthetics.

---

## Features

- **Full registry scan** — Enumerates all Windows services and their binary paths
- **Exploit vector analysis** — Identifies every interceptable path segment for each vulnerable service
- **Permission checks** — Verifies write access on each directory to confirm exploitability
- **Risk classification** — Grades each finding: `LOW`, `MEDIUM`, `HIGH`, `CRITICAL`
- **Privilege context** — Displays service start account and admin rights for impact assessment
- **Export to file** — Optionally saves results for reporting or further analysis
- **Color-coded UI** — Clear visual legend distinguishing exploitable, accessible, and inaccessible paths

---

## Risk levels

| Level | Meaning |
|---|---|
| `[OK]` / `WRITE ACCESS` | Exploitable — directory is writable |
| `ACCESS DENIED` | Vulnerability present but directory not writable |
| `DIR NOT FOUND` | Directory doesn't exist |
| `CRITICAL` | Immediate security threat |
| `ERROR` | System error or failure |

---

## Requirements

- Windows OS
- `.NET` runtime (or standalone executable — no install required)
- Administrator rights recommended *(some services may be inaccessible without them)*

---

## Installation & Usage

```sh
git clone https://github.com/F0X-hack/UnquotedFinder.git
cd UnquotedFinder
UnquotedFinder.exe
```

No dependencies to install. Just run the executable.

---

## Output example

```
[SYS] Timestamp: 2025-12-09 13:44:34
[SYS] Target: Windows Registry Services
[SYS] Mission: Detect Unquoted Service Paths

[WARN] Not running as administrator. Some services may be inaccessible.
[SCAN] Penetrating Windows Registry...
[SCANNING] ████████████████████ 100.0% 

[DATA] Services scanned: 779
[VULN] Vulnerabilities found: 1

[!] CRITICAL VULNERABILITIES DETECTED [!]

[#01] Sunshine Service
  Service Name:   SunshineService
  Binary Path:    C:\Program Files\Sunshine\tools\sunshinesvc.exe

  EXPLOIT VECTORS:
    └─ Vector #1
       Target:          C:\Program.exe
       Directory:       C:\
       Status:          [!] ACCESS DENIED
       Risk Level:      [LOW RISK]
       Start Name:      LocalSystem
       Privilege Level: SYSTEM (Maximum)
       Admin Rights:    GUI
       Risk if Exploited: Critical (Privilege Escalation to SYSTEM)
```

---

## How unquoted service path works

When a Windows service binary path contains spaces and is **not** quoted, the Service Control Manager tries to resolve the executable by splitting on each space. For example:

```
C:\Program Files\My App\service.exe
```

Windows will attempt to execute, in order:
1. `C:\Program.exe`
2. `C:\Program Files\My.exe`
3. `C:\Program Files\My App\service.exe`

If an attacker can write `C:\Program.exe`, it will be executed as the service's user — potentially `SYSTEM`.

---

## Project structure

```
UnquotedFinder/
├── UnquotedFinder.exe     # Standalone executable
└── README.md
```

---

## Credits

Tool developed by **[FoXhack](https://github.com/F0X-hack)**.

Vulnerability class reference: [MITRE CWE-428](https://cwe.mitre.org/data/definitions/428.html) — Unquoted Search Path or Element.

---

## Legal disclaimer

> This tool is intended for **educational purposes and authorized security assessments only.**  
> Running it against systems you do not own or have explicit permission to test is illegal.  
> The author takes no responsibility for unethical or unlawful use.

---

<div align="center">


</div>
