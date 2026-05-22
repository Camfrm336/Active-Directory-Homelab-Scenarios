# Active-Directory-Homelab-Scenarios

This document covers hands-on practice scenarios performed inside my Active Directory home lab. Each scenario simulates a real help desk ticket and documents the steps taken to resolve it, tools used, and key takeaways. All scenarios were performed on a Windows Server 2019 domain controller and a domain-joined Windows 10 client VM.

## Table of Contents
1. [Account Management](1-account-manangement)
2. Group Policy


## 1. Account Management

### Scenario 1.1 - Unlock a Locked Out User Account
Ticket simulation: User calls in saying they cannot log in and are receiving an "account locked" error.
Steps taken:

1. Opened Active Directory Users and Computers (ADUC) on the DC
2. Right-clicked the domain and selected Find to search for the user by name
3. Double-clicked the user account and navigated to the Account tab
4. Checked the "Unlock account" checkbox and clicked Apply
5. Confirmed with the user that login was successful

Commands used:
```powershell 
# Alternative method via PowerShell
Unlock-ADAccount -Identity jdoe
```
### What caused the lockout:
Intentionally triggered by entering the wrong password more than the lockout threshold (configured in Default Domain Policy under Account Lockout Policy).
📸 Screenshot: ADUC showing the user account with the Account tab open and the unlock checkbox visible
📸 Screenshot: PowerShell confirmation that the account was unlocked
Key takeaway: Always check the Account Lockout Policy threshold in Group Policy so you can explain to users why their account locked. Also check Event Viewer for Event ID 4740 to see which machine triggered the lockout.
