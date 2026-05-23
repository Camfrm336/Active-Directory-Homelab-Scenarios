# Active-Directory-Homelab-Scenarios

This document covers hands-on practice scenarios performed inside my Active Directory home lab. Each scenario simulates a real help desk ticket and documents the steps taken to resolve it, tools used, and key takeaways. All scenarios were performed on a Windows Server 2019 domain controller and a domain-joined Windows 10 client VM.

## Table of Contents
1. [Account Management](1-account-manangement)
2. Group Policy


## 1. Account Management

### Scenario 1.1 - Unlock a Locked Out User Account
Ticket simulation: User calls in saying they cannot log in and are receiving an "account locked" error.  
### Steps taken:

1. Opened Active Directory Users and Computers (ADUC) on the DC
2. Right-clicked the domain and selected Find to search for the user by name
3. Double-clicked the user account and navigated to the Account tab
4. Checked the "Unlock account" checkbox and clicked Apply
5. Confirmed with the user that login was successful

Commands used:
```powershell 
# Alternative method via PowerShell
Unlock-ADAccount -Identity cmcmaster
```
### What caused the lockout:
Intentionally triggered by entering the wrong password more than the lockout threshold (configured in Default Domain Policy under Account Lockout Policy).  
<img height="300" alt="locked-out-user" src="https://github.com/user-attachments/assets/708ac391-26ff-4893-b91c-8eb540ad6875" />  
📸 Screenshot: Proof of user account lockout  
<img height="300" alt="locked-out-user-settings" src="https://github.com/user-attachments/assets/596bdc79-1e1c-4add-afbf-5e6af5a7ddfa" />  
📸 Screenshot: ADUC showing the user account with the Account tab open and the unlock checkbox visible  
### Key takeaway:  
Always check the Account Lockout Policy threshold in Group Policy so you can explain to users why their account locked. Also check Event Viewer for Event ID 4740 to see which machine triggered the lockout.

### Scenario 1.2 - Password Reset  
### Ticket simulation: User forgot their password and cannot log in.  
### Steps taken:  

1. Verified the caller's identity before resetting (name, employee ID, manager name)
2. Located the user in ADUC
3. Right-clicked the user account and selected Reset Password
4. Set a temporary password meeting complexity requirements
5. Checked "User must change password at next logon"
6. Communicated the temporary password to the user securely
7. Confirmed the user was able to log in and set a new password

Commands used:
``` powershell
# PowerShell method
Set-ADAccountPassword -Identity cmcmaster -Reset -NewPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force)
Set-ADUser -Identity cmcmaster -ChangePasswordAtLogon $true
```
<img height="300" alt="password-reset" src="https://github.com/user-attachments/assets/5103c9f1-fb3c-4676-bfee-cb8562f1d1df" />  

📸 Screenshot: ADUC Reset Password dialog with "User must change password at next logon" checked

<img height="300" alt="change-password-on-signin" src="https://github.com/user-attachments/assets/808d8337-db64-4ab0-9708-5ad88ac23d31" />  

📸 Screenshot: Affected user being prompted to change password on sign-in  

<img height="300" alt="successful-password-change" src="https://github.com/user-attachments/assets/7166a0b1-2e01-4fac-b711-4dc9883df935" />  

📸 Screenshot: Succesful password change  

### Key takeaway:  
Always verify identity before resetting. Always force a password change at next logon so the admin never knows the user's permanent password. Temp passwords are not allowed if passwords do not expire.
