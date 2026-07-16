# Active Directory Identity & Access Troubleshooting Lab
A home-lab project in a simulated 200-user healthcare Active Directory
environment. I built the environment and resolved a realistic identity/access incident
(Ticket NMG-0047) end-to-end — from diagnosis through resolution, verification, and
prevention.

## Problem Statement
A newly onboarded Operations Coordinator (Jane Cooper) could log in successfully but
received Access Denied on all Operations shared resources. Her desktop environment
— group policies, mapped drives, and restrictions — also behaved differently from the
rest of her team, while no other Operations team member was affected. The issue
persisted after multiple restarts.
Left unresolved, this blocked a new hire from working during her first week and pointed
to a possible gap in the account-provisioning process.

## Solutions Overview
I resolved the issue by comparing the affected account against a known-good teammate
(Carlos Rivera) to isolate exactly what was different — investigating fully before making
any change.
The comparison surfaced two separate root causes from a single sloppy account
setup:
Wrong Organizational Unit (OU): Jane was placed in the HR OU instead of the
Operations OU, so the wrong Group Policy applied to her account (explaining the
different desktop, drives, and policies).
Missing security group: Jane was not a member of the Operations-Users group,
which is what grants access to Operations resources (explaining the Access Denied).
She had also been added to HR-Users in error.

Fixes applied (one per root cause):
1. Moved Jane from the HR OU to the Operations OU so the correct Group Policy
applies.
2. Added Jane to the Operations-Users security group to restore resource access.
3. Removed Jane from HR-Users to eliminate unintended HR access.
Verified both symptoms cleared — correct OU, correct group membership, correct
policies applied after gpupdate /force and re-login, and confirmed access to
Operations resources.
Key lesson: The obvious symptom (Access Denied) hid a second root cause
(OU/GPO). Fixing only the visible one would have left the ticket half-resolved — so I
always verify the full fix, not just the first thing I find.

## Tools Used
Active Directory Domain Services (AD DS) — Windows Server domain controller
hosting the simulated environment
Active Directory Users and Computers (ADUC) — inspecting/adjusting OU
placement and group membership
Group Policy Management Console (GPMC) — understanding how GPO applies by
OU
Command line — gpupdate /force to reapply policy during verification
Windows client VM — reproducing and confirming the end-user experience
Virtualization (VMware / VirtualBox / Hyper-V) — hosting the lab environment
PowerShell (planned next step) — New-ADUser script to automate consistent
provisioning

## Project Timeline
Stage What I did
1. Build Stood up a simulated 200-user healthcare AD environment (OUs, users,

security groups, GPOs)

2. Intake Received Ticket NMG-0047 and documented the reported symptoms

without jumping to conclusions

3.
Investigate

Compared the affected account against a known-good teammate to
isolate differences

4. Root
cause

Identified two issues via Five Whys analysis: wrong OU and missing
security group

5. Resolve Applied one fix per root cause (OU move, group add, erroneous group

removal)

6. Verify Confirmed both symptoms cleared after gpupdate /force and re-login
7.
Document Wrote up the incident and recommended prevention measures

Total resolution time: ~40 minutes.

## Key Accomplishments
Diagnosed and resolved a realistic AD access incident end-to-end in ~40 minutes.
Identified two distinct root causes where a less thorough approach would have
caught only one — using a known-good baseline comparison instead of guessing.
Restored the user to full functionality and verified both symptoms cleared, not
just the obvious one.
Applied a repeatable troubleshooting method: investigate → root-cause → fix →
verify → document.
Recommended prevention: a standardized onboarding checklist plus a PowerShell
( New-ADUser ) provisioning script to enforce correct OU and group assignment
automatically — turning a one-time fix into a systemic safeguard.


