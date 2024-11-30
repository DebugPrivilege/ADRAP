# Summary

The `PASSWD_NOTREQD` flag in Active Directory is a setting within the userAccountControl attribute that allows an account to exist without requiring a password. When this flag is enabled, the account can be created or maintained without meeting any password requirements.

## Remediation

1. Ensure that this script is executed on a domain-joined machine with the Active Directory PowerShell module installed. While Domain Admin (DA) privileges are not strictly required to run the script, having temporary DA access is recommended to avoid obstacles caused by insufficient permissions.

2. Download the script, then execute the following command to list all accounts, including computer accounts, that have the PASSWD_NOTREQD flag enabled:

```
.\ManagePasswdNotReqd.ps1 -ListAccounts
```

![image](https://github.com/user-attachments/assets/7eb98414-9acc-4da9-a73a-e479bdcb8595)
