**🚀 CyberArk REST API: Automated Bulk Onboarding**

**Executive Summary**

This project automates the critical "Day 0" identity lifecycle process for Privileged Access Management (PAM). By leveraging the CyberArk PAS REST API and PowerShell, this tool replaces manual, error-prone GUI data entry with a scalable, code-driven intake pipeline.

**The Metric:** Reduced onboarding time for 50+ privileged identities from 3+ hours (Manual) to <30 seconds (Automated).

**🛡️ Architecture & Logic**

The automation follows a strict "Trust-but-Verify" logic flow, connecting the "Core" (Directory Data) to the "Penthouse" (Privileged Vault).

Authentication (The Handshake):

Initiates a secure POST request to /auth/CyberArk/Logon.

Retrieves a temporal Session Token (wristband) to validate subsequent requests without exposing credentials repeatedly.

The Loop (The Engine):

Parses a structured CSV inventory (onboard.csv).

Iterates through each identity row, constructing a dynamic JSON payload.

The Payload (The Injection):

Fires a POST request to /PasswordVault/API/Accounts for each user.

Result: Instant object creation in the Vault with correct Safe, Platform, and Address assignments.

**📂 The Data Structure (The Fuel)**

The script relies on a standardized CSV input file. This decouples the code from the data, allowing non-technical teams (HR/Managers) to provide lists without touching the PowerShell logic.

onboard.csv Format: username,address,safe,platform Admin_John,192.168.1.50,Tier1_Operations,UnixSSH Admin_Sarah,192.168.1.51,Tier1_Operations,WinServerLocal

📸 Evidence 1: The Input Source

Figure 1: The raw intake file prepared for ingestion.
<img width="462" height="132" alt="Screenshot 2026-01-12 140116" src="https://github.com/user-attachments/assets/3f366ec2-8294-4530-ac2f-57854cb63106" />


**💻 Execution Output**

The script provides real-time console feedback, distinguishing between Success (Green) and API Failures (Yellow/Red) (e.g., "Account already exists").

Step 1: Secure Authentication Figure 2: Secure credential handling using PowerShell Get-Credential.
<img width="668" height="394" alt="Screenshot 2026-01-12 135531" src="https://github.com/user-attachments/assets/371541a8-3b1d-4fdb-9b9b-4a8d4d070f51" />


Step 2: JSON Payload Delivery Figure 3: Successful API Handshake and iterative object creation. Note the platformID and safeName assignment.

<img width="676" height="270" alt="Screenshot 2026-01-12 135627" src="https://github.com/user-attachments/assets/97e8ea16-cd33-465b-b5a2-e8cb7f636db4" />


**✅ Verification (The Vault)**

Post-execution validation confirms that objects were correctly instantiated in the target Safes.

**The Target Safe:** Figure 4: The 'Tier1_Operations' Safe designated for the new accounts.

<img width="790" height="599" alt="Screenshot 2026-01-12 140058" src="https://github.com/user-attachments/assets/f2c93a27-7862-46f4-a9fe-fe65d6900b8b" />


**The Final Result:** Admin_John, Admin_Mike, and Admin_Sarah successfully vaulted and ready for management.


**🧠 Engineering Takeaways**

**Idempotency:** The script design allows it to handle "Account Already Exists" errors gracefully without crashing the entire pipeline.

**Security:** Credentials are used only once to acquire the Token; the Token is used for the heavy lifting.

**Scalability:** This logic scales linearly. Whether onboarding 5 users or 5,000, the engineering effort remains constant.
