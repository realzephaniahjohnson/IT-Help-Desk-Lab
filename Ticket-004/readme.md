# Ticket 004 - Unable to Access Shared Folder

## Issue

User reported being unable to access the company shared folder located at `\\dc01\Company Shared`.

---

## Environment

- Windows Server 2025 Datacenter
- Windows 11
- Active Directory Domain Services
- SMB File Sharing
- Domain: `zephlab.local`
- File Server: `dc01`
- User Account: `jsmith`

---

## Symptoms

- User was unable to open the `Company Shared` network folder.
- Windows displayed a permissions-related error.
- User received the message:

  **You do not have permission to access `\\dc01\Company Shared`.**

---

## Investigation

- Reproduced the issue by attempting to access `\\dc01\Company Shared`.
- Confirmed Windows returned an access denied / permissions error.
- Opened Command Prompt on the affected workstation.
- Used `ping dc01` to verify connectivity to the file server.
- Confirmed `dc01.zephlab.local` resolved successfully to `172.16.0.4`.
- Received replies from the server with **0% packet loss**.
- Used `whoami` to verify the affected user was signed in as `zephlab\jsmith`.
- Determined that basic network connectivity and domain authentication were functioning correctly.
- Isolated the issue to permissions associated with the shared folder.

---

## Resolution

- Corrected the required permissions for the `jsmith` user to access the `Company Shared` folder.
- Retested access from the Windows 11 workstation.
- Successfully opened `\\dc01\Company Shared`.
- Confirmed the user could access the network share without receiving the previous permissions error.

---

## Root Cause

The user account did not have the required permissions to access the `Company Shared` network folder.

Network connectivity to the file server was functioning normally, confirming the issue was related to access permissions rather than DNS, connectivity, or server availability.

---

## Evidence

### Access Denied

![Access Denied](Access%20Denied.png)

Attempted to access `\\dc01\Company Shared` and received a Windows permissions error indicating that the user did not have access to the shared folder.

---

### Network Connectivity Verification

![Ping Confirmation](ping%20confirmation.png)

Used `ping dc01` to verify communication with the file server. The hostname successfully resolved to `172.16.0.4`, and all four packets were received with **0% packet loss**.

Also used `whoami` to confirm the affected user was authenticated as `zephlab\jsmith`.

---

### Granted Access Confirmation

![Granted Access Confirmation](granted%20access%20confirmation.png)

Retested the network share after permissions were corrected and successfully opened `\\dc01\Company Shared`, confirming the issue was resolved.

---

## Skills Demonstrated

- Windows 11 Administration
- Windows Server Administration
- Active Directory Domain Services
- SMB File Sharing
- Shared Folder Permissions
- Network Share Troubleshooting
- Windows File Explorer
- Command Prompt
- Ping
- DNS Name Resolution
- User Authentication Verification
- Access Control Troubleshooting
- Root Cause Analysis
- Help Desk Troubleshooting
- Ticket Documentation
