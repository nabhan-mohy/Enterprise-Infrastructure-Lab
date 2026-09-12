# 1.2 — Windows Users, Groups & Privileges

> **Lab Area:** Windows Administration  
> **Module:** 1.2 — Users, Groups & Privileges  
> **Environment:** Virtualized Enterprise IT Support Lab  
> **Platform:** Microsoft Windows  
> **Primary Tool:** Windows PowerShell

---

## 1. Module Overview

User and group administration is a fundamental responsibility of an IT support engineer and Windows system administrator.

In this module, local Windows user accounts and security groups were reviewed and configured in a controlled laboratory environment.

The exercise simulated a basic enterprise access-management scenario where an employee account is created and assigned to appropriate organizational groups.

The following activities were performed:

- Reviewed existing local user accounts.
- Created a new local employee account.
- Assigned an appropriate account description.
- Created department/role-based local groups.
- Added the employee account to appropriate groups.
- Verified group membership.
- Reviewed account status.
- Configured the employee account password.
- Validated the resulting user and group configuration.

The purpose was to demonstrate practical Windows identity and access-management tasks that are commonly encountered in IT support environments.

---

# 2. Objectives

The objectives of this module were to:

- Understand Windows local user administration.
- Review existing local accounts.
- Create a local employee account.
- Configure an account description.
- Create role-based security groups.
- Assign users to appropriate groups.
- Verify group membership.
- Check whether an account is enabled.
- Review password requirements.
- Configure a temporary laboratory password.
- Validate the final user configuration.
- Understand the relationship between users, groups, and privileges.

---

# 3. Lab Scenario

A simulated organization requires a new employee account for an IT support technician.

The account will be represented by:

```text
Username:
employee01
```

The account description is:

```text
IT Support Technician
```

The laboratory also requires role-based groups representing different organizational functions:

```text
Employees
IT-Support
SOC-Analysts
```

The employee will be assigned to the appropriate groups.

### Intended access model

```text
                    Windows Workstation
                           |
                           |
                    Local User Account
                           |
                      employee01
                           |
              +------------+------------+
              |                         |
              v                         v
          Employees                IT-Support
                                     
```

The `SOC-Analysts` group was also created as part of the simulated organizational structure but was not assigned to `employee01` during this exercise.

---

# 4. Review Existing Local Users

Before creating a new account, the existing local users were reviewed.

### Command

```powershell
Get-LocalUser
```

The baseline system contained several built-in Windows accounts, including:

```text
Administrator
DefaultAccount
Guest
kewex
WDAGUtilityAccount
```

The existing accounts were reviewed before making any changes.

This is an important administrative practice because an administrator should understand the existing identity configuration before creating or modifying accounts.

---

### 📸 Screenshot — Existing Local Users

<img src="screenshots/01-existing-local-users.png"
     width="700"
     alt="PowerShell showing existing Windows local users">

**Evidence:** PowerShell output from `Get-LocalUser` showing the existing local accounts.

---

# 5. Create Employee Account

A new local user account was created for the simulated IT support employee.

### Command

```powershell
New-LocalUser -Name "employee01" -Description "IT Support Technician"
```

PowerShell prompted for a password during account creation.

The resulting account was:

```text
Name:
employee01

Enabled:
True

Description:
IT Support Technician
```

The account was created specifically for the laboratory's simulated employee-access scenario.

---

### 📸 Screenshot — Employee Account Creation

<img src="screenshots/02-create-employee01.png"
     width="700"
     alt="PowerShell creating employee01 local user account">

**Evidence:** PowerShell showing creation of the `employee01` account and the resulting account information.

---

# 6. Validate Employee Account

After creating the account, the account was independently queried.

### Command

```powershell
Get-LocalUser employee01
```

The account was returned successfully.

The account information showed:

```text
Name:
employee01

Enabled:
True

Description:
IT Support Technician
```

This confirmed that the account creation operation was successful.

---

### 📸 Screenshot — Employee Account Validation

<img src="screenshots/03-employee01-validation.png"
     width="700"
     alt="PowerShell validating employee01 local user">

**Evidence:** `Get-LocalUser employee01` confirming the new account.

---

# 7. Review User Account Properties

Additional properties were reviewed using:

```powershell
Get-LocalUser employee01 |
Select-Object Name, Enabled, Description, LastLogon
```

The expected account state was:

```text
Name:
employee01

Enabled:
True

Description:
IT Support Technician
```

The account properties provide useful information for help-desk and system-administration tasks.

For example, when investigating an authentication problem, an administrator may need to determine:

- Whether the account exists.
- Whether the account is enabled.
- When the account last logged in.
- Whether the account has the expected description.
- Which groups the account belongs to.

---

### 📸 Screenshot — Employee Account Properties

<img src="screenshots/04-employee01-properties.png"
     width="700"
     alt="PowerShell displaying employee01 account properties">

**Evidence:** PowerShell showing the account name, enabled state, description, and login information.

---

# 8. Create Role-Based Groups

The next step was to create local groups representing different organizational roles.

The following groups were created:

```text
IT-Support
SOC-Analysts
Employees
```

These groups simulate common enterprise organizational structures.

---

## 8.1 IT-Support Group

### Command

```powershell
New-LocalGroup -Name "IT-Support" -Description "IT Support personnel"
```

The group was created to represent personnel responsible for technical support and workstation administration.

---

## 8.2 SOC-Analysts Group

### Command

```powershell
New-LocalGroup -Name "SOC-Analysts" -Description "SOC analysts"
```

The group represents security operations personnel in the simulated environment.

---

## 8.3 Employees Group

### Command

```powershell
New-LocalGroup -Name "Employees" -Description "Standard employees"
```

The group represents standard organizational employees.

---

### 📸 Screenshot — Role-Based Group Creation

<img src="screenshots/05-create-role-groups.png"
     width="700"
     alt="PowerShell creating IT-Support SOC-Analysts and Employees groups">

**Evidence:** PowerShell showing creation of the role-based local groups.

---

# 9. Verify Local Groups

After creating the groups, the local group inventory was reviewed.

### Command

```powershell
Get-LocalGroup
```

The command displayed the available Windows local groups, including the newly created groups:

```text
Employees
IT-Support
SOC-Analysts
```

The built-in Windows groups were also present.

This validation confirms that the groups were successfully created.

---

### 📸 Screenshot — Local Group Inventory

<img src="screenshots/06-local-group-inventory.png"
     width="700"
     alt="PowerShell showing local Windows groups">

**Evidence:** `Get-LocalGroup` output showing the newly created organizational groups.

---

# 10. Add Employee to Employees Group

The new employee account was added to the standard employee group.

### Command

```powershell
Add-LocalGroupMember -Group "Employees" -Member "employee01"
```

This establishes `employee01` as a member of the simulated standard employee population.

---

# 11. Add Employee to IT-Support Group

Because the account represents an IT Support Technician, it was also added to the IT support group.

### Command

```powershell
Add-LocalGroupMember -Group "IT-Support" -Member "employee01"
```

This associates the employee account with the simulated IT support role.

The account was **not** added to the `SOC-Analysts` group during this exercise.

This demonstrates role-based group assignment rather than assigning every available role to the same account.

---

### 📸 Screenshot — Group Membership Assignment

<img src="screenshots/07-add-group-membership.png"
     width="700"
     alt="PowerShell adding employee01 to Employees and IT-Support groups">

**Evidence:** PowerShell showing `employee01` being added to the appropriate local groups.

---

# 12. Verify Employees Group Membership

The membership of the `Employees` group was checked using:

```powershell
Get-LocalGroupMember -Group "Employees"
```

The resulting membership included:

```text
employee01
```

This confirms that the user was successfully assigned to the standard employee group.

---

### 📸 Screenshot — Employees Group Membership

<img src="screenshots/08-employees-group-membership.png"
     width="700"
     alt="PowerShell showing employee01 as a member of Employees">

**Evidence:** `Get-LocalGroupMember -Group "Employees"` confirming membership.

---

# 13. Verify IT-Support Group Membership

The IT support group was checked using:

```powershell
Get-LocalGroupMember -Group "IT-Support"
```

The resulting membership included:

```text
employee01
```

This confirms that the employee account was successfully assigned to the IT Support role.

---

### 📸 Screenshot — IT-Support Group Membership

<img src="screenshots/09-it-support-group-membership.png"
     width="700"
     alt="PowerShell showing employee01 as a member of IT-Support">

**Evidence:** `Get-LocalGroupMember -Group "IT-Support"` confirming the employee's IT Support group membership.

---

# 14. Verify SOC-Analysts Group

The SOC analyst group was created but the employee account was not assigned to it.

This reflects a basic principle of role-based access control:

> Users should receive only the group memberships required for their job role.

The group can be used later in the lab when implementing security operations or access-control scenarios.

---

### 📸 Screenshot — SOC-Analysts Group

<img src="screenshots/10-soc-analysts-group.png"
     width="700"
     alt="PowerShell showing the SOC-Analysts local group">

**Evidence:** PowerShell showing the created `SOC-Analysts` group.

---

# 15. Password Configuration

A temporary laboratory password was configured for `employee01`.

### Command

```powershell
$NewPassword = Read-Host "Enter temporary password" -AsSecureString

Set-LocalUser -Name "employee01" -Password $NewPassword
```

The password was entered interactively as a secure string.

No password was written into the command history or documentation.

---

## Security Practice

Passwords must never be stored in:

```text
README.md
Screenshots
Git repositories
Command-output files
Issue descriptions
Public documentation
```

The laboratory password should remain private and should not be uploaded to GitHub.

---

### 📸 Screenshot — Password Configuration

<img src="screenshots/11-password-configuration.png"
     width="700"
     alt="PowerShell securely configuring the employee01 password">

**Evidence:** PowerShell showing secure password configuration without exposing the actual password.

> **Important:** Make sure the screenshot does not contain the password or any sensitive credential information.

---

# 16. Validate Password Requirement

The account configuration was checked using:

```powershell
Get-LocalUser employee01 |
Select-Object Name, Enabled, PasswordRequired
```

The account was shown as:

```text
Name             : employee01
Enabled          : True
PasswordRequired : False
```

The observed value is documented exactly as captured during the laboratory exercise.

This demonstrates why account properties should be validated after configuration changes rather than assuming that the intended configuration was successfully applied.

---

### 📸 Screenshot — Password Requirement Validation

<img src="screenshots/12-password-validation.png"
     width="700"
     alt="PowerShell showing employee01 password configuration">

**Evidence:** PowerShell output showing the account's enabled state and password requirement.

---

# 17. Final User Configuration

After the account and group configuration was completed, the resulting user configuration was:

```text
User:
employee01

Description:
IT Support Technician

Enabled:
True

Groups:
Employees
IT-Support
```

The account was not assigned to:

```text
SOC-Analysts
```

---

# 18. Final Group Structure

The simulated organizational group structure was:

```text
Employees
│
└── employee01

IT-Support
│
└── employee01

SOC-Analysts
│
└── No employee01 membership
```

This provides a simple role-based access model for the laboratory.

---

# 19. User and Group Validation

The following commands can be used to reproduce the final validation.

### Check user

```powershell
Get-LocalUser employee01
```

### Check user properties

```powershell
Get-LocalUser employee01 |
Select-Object Name, Enabled, Description, LastLogon
```

### Check Employees membership

```powershell
Get-LocalGroupMember -Group "Employees"
```

### Check IT-Support membership

```powershell
Get-LocalGroupMember -Group "IT-Support"
```

### Check SOC-Analysts membership

```powershell
Get-LocalGroupMember -Group "SOC-Analysts"
```

### List all local groups

```powershell
Get-LocalGroup
```

---

### 📸 Screenshot — Final Validation

<img src="screenshots/13-final-user-group-validation.png"
     width="700"
     alt="PowerShell final validation of Windows user and group configuration">

**Evidence:** Final PowerShell validation showing the configured user and group memberships.

---

# 20. Troubleshooting Scenario

## Scenario: Employee Cannot Access IT Support Resources

A common IT support scenario is:

> An employee reports that they cannot access a resource available to the IT Support team.

The first step is to verify whether the user's account belongs to the expected security group.

### Check the group

```powershell
Get-LocalGroupMember -Group "IT-Support"
```

If `employee01` is missing, the account may not have the expected group membership.

The administrator can then verify the user:

```powershell
Get-LocalUser employee01
```

and check other group memberships.

---

## Troubleshooting Workflow

```text
User reports access problem
          |
          v
Verify account exists
          |
          v
Check account enabled state
          |
          v
Check group membership
          |
          v
Compare membership with job role
          |
          v
Correct group assignment if authorized
          |
          v
Re-test access
          |
          v
Document the change
```

This workflow demonstrates a practical approach to access-related IT support tickets.

---

# 21. Role-Based Access Control Concept

The exercise demonstrates a simplified form of role-based access control.

Instead of assigning permissions individually to every user:

```text
User → Permission
```

an organization can use:

```text
User → Group → Permission
```

For example:

```text
employee01
     |
     +---- Employees
     |
     +---- IT-Support
```

The group represents the employee's organizational role.

This approach simplifies administration because permissions can be associated with groups rather than managed separately for every user.

---

# 22. Least Privilege

The account was assigned only to the groups required for the simulated role.

The user was not automatically placed into every available administrative or security group.

This reflects the principle of:

> **Least Privilege**

Least privilege means users should receive only the access required to perform their assigned responsibilities.

This reduces unnecessary access and limits the potential impact of a compromised account.

---


**Enterprise Infrastructure Lab**  
*Hands-on Windows & Linux administration, networking, troubleshooting, monitoring, security, and IT support laboratory.*
