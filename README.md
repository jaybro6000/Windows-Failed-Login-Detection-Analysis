# Windows-Failed-Login-Detection-Analysis

## Overview
This project demonstrates forensic log analysis on Windows Security Event Logs to investigate failed login attempts (**Event ID 4625**). By transitioning from legacy command-line tools to native **PowerShell**, raw `.evtx` log files were parsed, analyzed for potential brute-force patterns, correlated with authentication events, and translated into clean reports for non-technical stakeholders.

---

## 1. Initial Log Inspection & Tool Pivoting
The investigation began by reviewing raw security events using **Windows Event Viewer** to isolate failed logon events (Event ID 4625).

![Windows Event Viewer Filtered Logs] <img width="1366" height="676" alt="SOC5" src="https://github.com/user-attachments/assets/7bc11ab2-c1ac-43d3-906c-600c58f79b12" />


Legacy tools like Microsoft `LogParser.exe` often encounter syntax and parsing issues with modern `.evtx` binary formats.

![Legacy LogParser Syntax Testing](<img width="1366" height="768" alt="SOC4" src="https://github.com/user-attachments/assets/9fd8e3a9-08f0-41b2-bd39-ecbe61fa44bb" />
)

---

## 2. Automated Extraction via PowerShell
To bypass legacy syntax limitations, a custom PowerShell script was executed to extract key XML attributes—including `TimeCreated`, `TargetUserName`, `LogonType`, `IpAddress`, and `WorkstationName`—and export them directly into a structured CSV format.

![PowerShell Log Extraction Script](<img width="1337" height="636" alt="SOC8" src="https://github.com/user-attachments/assets/89d432a1-d3e4-4a7c-9024-43a96bed9062" />
)

The extracted output was verified directly from the terminal to ensure complete telemetry collection.

![Terminal CSV Verification](<img width="460" height="589" alt="SOC9" src="https://github.com/user-attachments/assets/85bbd481-fc96-43eb-92b8-e7533918fad8" />
)

---

## 3. Event Correlation & Analysis
To detect potential brute-force or password-spraying attacks, a correlation script cross-referenced failed logons (**Event ID 4625**) against successful logons (**Event ID 4624**).

![Event Correlation Script](<img width="1234" height="650" alt="SOC10" src="https://github.com/user-attachments/assets/a449bf20-4a91-4919-96ab-ba0fd921e9e7" />
)

---

## 4. HTML Report Generation & Stakeholder Delivery
To present raw technical telemetry to non-technical stakeholders, PowerShell transformed numeric logon types (e.g., `LogonType 2` $\rightarrow$ `Physical Keyboard (Interactive)`) into an executive HTML summary report.

![HTML Summary Generation Script](<img width="1304" height="570" alt="SOC12" src="https://github.com/user-attachments/assets/56c35841-39e0-4be7-85e3-6d855f3cc0e9" />
)

The finalized report formats telemetry into a readable audit document.

![Final Human-Readable HTML Report](<img width="564" height="498" alt="SOC11" src="https://github.com/user-attachments/assets/c288afe0-847c-4c5d-8afe-e2847c2a7d65" />
)

---

## Technical Summary & Findings
* **Event Analyzed:** Windows Audit Failure (**Event ID 4625**)
* **Logon Type Detected:** Type 2 (*Local Interactive / Keyboard*)
* **Source Address:** `127.0.0.1` (*Local Loopback*)
* **Risk Assessment:** Low / Internal Testing. Activity represents local console authentication failures rather than an automated external network attack.
