# What is a service account?

A service account in Active Directory (AD) is a type of account created to run applications, services, or automated tasks instead of regular user accounts. These accounts are typically not used for interactive logins like normal user accounts and are often tied to a specific application or service, such as a web server, database, or scheduled task.

## Types of service accounts

**Standard User Accounts:** 
- Regular user accounts in AD that are used as service accounts (not recommended due to manual password management and security concerns). This is the most common thing that you will be seeing in organizations.
   
**Managed Service Accounts (MSAs):**
- Automatically manage passwords.
- Can be used only on a single server.
  
**Group Managed Service Accounts (gMSAs):**
- A single account that can be used across multiple servers.
- Passwords are automatically rotated and managed by AD.

**Built-in Accounts:**
- Accounts like `LocalSystem`, `NetworkService`, and `LocalService` on Windows systems, used for running services without creating new accounts in AD.

## Remediation

This section will focus exclusively on remediating service accounts. Identifying whether a service account is still in use can be challenging without centralized logs to track its activity. However, we can address at minimum the stale service accounts that were created in the past but were never actively used. The following PowerShell script can be used to track down service accounts: https://gist.github.com/DebugPrivilege/bfe313a90e84fbf9d5d2d70d53db59c4

1. Download the script and execute it on a domain-joined machine. Start by importing the script using the command:

```
Import-Module .\GetServiceAccountDetails.ps1
```

![image](https://github.com/user-attachments/assets/3cdd3b81-0e39-4836-becf-cebbc4d946e9)

2. You can now invoke the `Get-ServiceAccountDetails` function, which includes the `-ServiceAccountPrefix` parameter. Use this to specify the prefix used for your service accounts (e.g., `svc_`, `sa_`, `srvc_`, `svc-`, etc.).

```
Get-ServiceAccountDetails -ServiceAccountPrefix "svc_"
```

![image](https://github.com/user-attachments/assets/ef0e11d7-626f-40d6-8f26-1e385b3769ec)

3. You can also use the `-CsvOutput` parameter to specify a file path where the results will be saved in .CSV format. This file can later be opened with tools like Timeline Explorer for further analysis.

```
Get-ServiceAccountDetails -ServiceAccountPrefix "svc_" -CsvOutput C:\Temp\Results.csv
```

![image](https://github.com/user-attachments/assets/0d9fd147-b2db-4773-bf9b-99ea6a84dd8e)

4. Open the CSV file with a tool like Timeline Explorer for further analysis

![image](https://github.com/user-attachments/assets/1feea511-8348-4aaf-88c5-75d970133914)


## Best Practices

If both the `lastLogon` and `lastLogonTimestamp` attributes are set to **Never**, it indicates that the account has never been used to log in since it was created. Cleaning up these accounts can improve security and reduce attack surface.

![image](https://github.com/user-attachments/assets/6f391e57-4c65-4291-9171-2f03415fa0ed)

If the `lastLogon` attribute is **1-1-1601** and the `lastLogonTimestamp` is **Never**, it suggests the same as the previous example. These accounts are unused and can be cleaned up to minimize the attack surface.

![image](https://github.com/user-attachments/assets/b14e7175-6230-4f9a-8edc-26911317d2c3)

1. Disable the accounts rather than deleting them immediately.
2. Set a review period, wait a few days, and monitor for any potential issues (though they are unlikely).
3. Document the actions, recording the steps taken and the decisions made during the cleanup process.
