# Windows Hardening
This document contains a number of items on how to secure your Microsoft Windows environment, this document, however is tailored towards an Active Directory environment.

## Enabling a secure baseline
* Ensure that [WSUS](https://technet.microsoft.com/en-us/library/hh852340(v=ws.11).aspx) is configured and deployed across all machines, and that updates are applied as soon as possible
* Ensure that all machines have been updated to Windows 10, or are being updated to Windows 10 as soon as possible
* Although being made end of life, look at deploying [EMET](https://support.microsoft.com/en-us/help/2458544/the-enhanced-mitigation-experience-toolkit) to all Workstations (this becomes end of life in July 2018)
* Implement [AppLocker](https://technet.microsoft.com/en-us/library/dd759117(v=ws.11).aspx) to block applications from running in user locations (such as home directory, profile path, temp, etc).
* Make sure that the [blocking of MS Office macros](https://blogs.technet.microsoft.com/mmpc/2016/03/22/new-feature-in-office-2016-can-block-macros-and-help-prevent-infection/) (Windows & Mac) on content downloaded from the Internet is enabled
* Make sure that all devices are running some form of endpoint security - anti-virus/anti-malware, host based firewall
* Make sure of centralised monitoring - ensure that event monitoring ([WEF](https://blogs.technet.microsoft.com/jepayne/2015/11/23/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem/)) is forwarded to a centalised location
* Limit the capability of running rogue applictions by blocking/restricting attachments via email/download:

	**Executables extensions:**
	(ade, adp, ani, bas, bat, chm, cmd, com, cpl, crt, hlp, ht, hta, inf, ins, isp, job, js, jse, lnk, mda, mdb, mde, mdz, msc, msi, msp, mst, pcd, pif, reg, scr, sct, shs, url, vb, vbe, vbs, wsc, wsf, wsh, exe, pif, etc.)
	
	**Office files that support macros** (docm, xlsm, pptm, etc.)

	Change default program for anything that opens with Windows scripting to notepad: bat, js, jse, vbe, vbs, wsf, wsh, etc.

## User & Group Accounts
* Ensure that the Domain Administrator group is limited to a select number of accounts - ideally no more than 5 people atmost
* Ensure that Enterprise and Schema Administrator accounts are restricted even further than Domain Admins
* Local Administrator access on workstations should not be used by default
* Ensure that users are only given access to Security Groups that they need access to
* Ensure that the local administrator account passwords are automatically changed ([Microsoft LAPS](https://www.microsoft.com/en-us/download/details.aspx?id=46899)) & remove any extra local admin accounts
* Configure the Group Policy Objects (GPO) to prevent local accounts from network authentication ([KB2871997](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-and-management-may-13,-2014))
* Use Managed Service Accounts for SAs when possible - Fine-Grained Password and Account Policy - [https://technet.microsoft.com/en-us/library/cc770842%28v=ws.10%29.aspx](https://technet.microsoft.com/en-us/library/cc770842%28v=ws.10%29.aspx) 
* Ensure all built-in groups but Administrator are denied from logging on to Domain Controllers user User Right Assignments. By default, Backup operators, Account operators can login to Domain Controllers, which is dangerous
* Add all admin accounts to [Protected Users group](https://technet.microsoft.com/en-us/library/dn466518%28v=ws.11%29.aspx) (requires Windows 2012 R2 DCs)

## Network
* Remove NetBIOS over TCP/IP
* Disable [LLMNR](https://en.wikipedia.org/wiki/Link-Local_Multicast_Name_Resolution)
* Disable [WPAD](https://en.wikipedia.org/wiki/Web_Proxy_Auto-Discovery_Protocol)
* Enforce [LDAP signing](https://technet.microsoft.com/en-us/library/dd941832%28v=ws.10%29.aspx)
* Enable [SMB signing](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/) (& encryption where possible)
* Windows 10, remove:
	* SMB 1.0/CIFS
	* Windows PowerShell 2.0
* Ensure all computers are talking NTLMv2 & Kerberos, deny [LM/NTLMv1](https://support.microsoft.com/en-us/help/2793313/security-guidance-for-ntlmv1-and-lm-network-authentication)