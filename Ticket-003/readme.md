# Ticket 003 - User Account Locked Out

## Issue

User reported being unable to authenticate to their domain account after multiple unsuccessful password attempts.

---

## Environment

- Windows Server 2025 Datacenter
- Windows 11
- Active Directory Domain Services
- Group Policy Management
- Domain: `zephlab.local`
- User Account: `jsmith`

---

## Symptoms

- User was unable to authenticate using their domain credentials.
- Authentication attempts returned an account lockout error.
- Account became locked after multiple invalid password attempts.

---

## Investigation

- Reviewed the domain Account Lockout Policy in Group Policy Management.
- Verified the **Account lockout threshold** was configured for **3 invalid logon attempts**.
- Verified the **Account lockout duration** was configured for **15 minutes**.
- Verified the account lockout counter was configured to reset after **10 minutes**.
- Attempted authentication using the `zephlab\jsmith` account.
- Received Windows error **1909**, confirming the referenced account was currently locked out.
- Reviewed the user account in Active Directory Users and Computers.
- Verified the `badPwdCount` attribute had reached **3**, matching the configured lockout threshold.
- Confirmed the account was marked as locked out in Active Directory.

---

## Resolution

- Opened the `jsmith` user account properties in Active Directory Users and Computers.
- Selected **Unlock account** to clear the account lockout.
- Applied the account changes.
- Retested authentication using the domain user account.
- Successfully authenticated as `zephlab\jsmith`.
- Verified successful authentication using the `whoami` command.

---

## Root Cause

The `jsmith` domain account was automatically locked after reaching the configured threshold of **3 invalid logon attempts**. The Active Directory account lockout policy functioned as expected.

---

## Evidence

### Account Lockout Policy

![Group Policy](Group%20Policy.png)

Verified the domain Account Lockout Policy was configured with a threshold of **3 invalid logon attempts**, a **15-minute lockout duration**, and a **10-minute account lockout counter reset period**.

---

### Command-Line Lockout Confirmation

![Lockout Confirmation CMD](Lockout%20Confirmation%20CMD.png)

Attempted to authenticate as `zephlab\jsmith` and received Windows **RUNAS Error 1909**, confirming the account was locked and could not log on.

---

### Active Directory Lockout Confirmation

![Lockout Confirmation ADUC](Lockout%20Confirmation%20ADUC.png)

Reviewed the `jsmith` account attributes in Active Directory and confirmed `badPwdCount` had reached **3**, matching the configured account lockout threshold.

---

### Account Unlock

![Unlocking Account](Unlocking%20Account.png)

Opened the user account properties and confirmed the account was locked. Selected **Unlock account** and applied the change to restore access.

---

### Successful Sign In

![Successful Sign In](Successful%20Sign%20In.png)

Successfully authenticated to the Windows 11 workstation after unlocking the account. Used `whoami` to verify the active user was `zephlab\jsmith`.

---

## Skills Demonstrated

- Active Directory Domain Services
- Active Directory Users and Computers
- User Account Administration
- Account Lockout Troubleshooting
- Group Policy Management
- Password and Authentication Troubleshooting
- Active Directory Attribute Analysis
- Windows Command Line
- Identity and Access Management
- Root Cause Analysis
- Help Desk Troubleshooting
- Ticket Documentation
