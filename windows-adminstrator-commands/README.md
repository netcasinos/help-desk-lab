Windows Administrator Commands Lab

This lab documents hands-on practice with elevated Windows Command Prompt and PowerShell commands used for account review, privilege verification, system administration, and troubleshooting.

Objective

The purpose of this lab was to verify administrator access and practice common Windows administrative commands in a controlled environment.

Environment

Windows 11

Administrator Command Prompt

Administrator PowerShell

Commands Practiced

Identify the Current User

whoami

Displays the account currently running the terminal.

Review Security Groups

whoami /groups

Displays the security groups and privilege assignments associated with the current account.

List Local User Accounts

net user

Displays the local accounts configured on the Windows computer.

System-managed accounts such as WDAGUtilityAccount and WsiAccount may also appear. Their presence is expected and does not indicate an unauthorized user.

Review the Local Administrators Group

net localgroup administrators

Displays the accounts and groups that belong to the local Administrators group.

Verify Elevated Administrator Access

([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)

The result returned:

True

This confirmed that PowerShell was running with elevated administrator privileges.

Evidence



Skills Demonstrated

Opening an elevated Windows terminal

Verifying administrator privileges

Reviewing local Windows accounts

Reviewing security-group membership

Interpreting Command Prompt and PowerShell output

Documenting administrative procedures

Status

Completed