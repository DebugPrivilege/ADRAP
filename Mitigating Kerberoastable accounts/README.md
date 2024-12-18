# Kerberoasting

Kerberoasting is an attack technique that targets the `servicePrincipalName` (SPN) attribute in Active Directory, which is used to associate service accounts with specific services. Attackers request Kerberos service tickets for accounts with SPNs, extract these tickets (encrypted with the service account's password hash), and attempt to crack them offline. If successful, they gain access to the service account, especially if the account has a weak or poorly managed password.

A practical first step to mitigating Kerberoastable accounts, beyond enforcing strong passwords on SPN accounts or using gMSAs, is to review all accounts with an SPN and verify whether the server referenced in the SPN is still active. A common scenario is discovering that the server where the service account was used has already been decommissioned, yet the service account remains enabled in Active Directory.

## Remediation

We will be using the `ManagedSPN.ps1` script to help manage and analyze Service Principal Names (SPNs) in Active Directory. When you use the `-Identity` parameter, it fetches details about the specified user, including when their password was last set, their last logon times, and the SPNs associated with the account. 

If you use `-RemoveAllSPN` or `-RemoveSPN`, the script removes either all or a specific SPN from the account. For each SPN, it extracts the server name, constructs a fully qualified domain name (FQDN) if necessary, and checks whether the server exists as a computer object in AD. It also tests the server's connectivity using `Test-NetConnection`, optionally checking a specified port. The results are displayed in a table showing the SPNs, server details, AD presence, and connectivity status. 

If you run the script with `-Kerberoast`, it lists all enabled accounts with SPNs, along with their `pwdLastSet` and `servicePrincipalName`, to help identify accounts that are be vulnerable to Kerberoasting. You can download the script here: https://gist.github.com/DebugPrivilege/ac668b894d26371723d669fdbfe50673

1. Before getting started, make sure you run the script on a domain-joined machine with the Active Directory PowerShell module installed.
2. We can run the `ManageSPN.ps1` script with the `-Kerberoast` parameter to list enabled user accounts that have an SPN, along with their `pwdLastSet` and `servicePrincipalName` attributes.

```
.\ManageSPN.ps1 -Kerberoast
```

![image](https://github.com/user-attachments/assets/8bd43d80-aea4-4655-a781-e831b461a822)

3. Once we have a list of all accounts with SPNs, we should prioritize accounts whose passwords haven't been rotated for an extended period, such as several years or even decades. These accounts should be addressed first to improve security, as it's very likely that they have a weak password.

When we specify the `-Identity` parameter, the script retrieves detailed information about the specified user's SPNs, password last set time, and recent logon activity, and performs checks on associated servers for existence in AD and connectivity.

```
.\ManageSPN.ps1 -Identity SVC_SCCM
```

![image](https://github.com/user-attachments/assets/e9225816-e742-43e2-920f-02c66c0e5381)

4. From the results, we can observe that the account has two SPNs. The output indicates that name resolution failed because the server was already decommissioned, and the associated computer object no longer exists in AD. This suggests it is safe to remove these SPNs, reset the account's password, and subsequently disable it. We can now use the `-RemoveAllSPN` parameter to remove all SPNs associated with this account, which mitigates the Kerberoasting attack.

```
.\ManageSPN.ps1 -Identity svc_sccm -RemoveAllSPN
```

![image](https://github.com/user-attachments/assets/d6273cd6-068e-4590-8ee9-cf34a7b3cd42)

## Best Practices

1. Start with accounts whose passwords haven't been rotated for a long time. First, check if the server linked to the SPN for the account is still active. If the server has already been decommissioned, it’s safe to remove the SPN and disable the account.
2. If you come across any "human" accounts with an SPN, that’s a misconfiguration and should be addressed immediately. The SPN must be removed from such accounts.
3. If the server does exist, but the account associated with an SPN for that server has never logged in, this is a strong indication that the account can be disabled. Additionally, if the `lastLogon` attribute has no value and the `lastLogonTimestamp` is set to a default date like `12/31/1600 4:00:00 PM`, it’s safe to disable the account as it appears unused.

![image](https://github.com/user-attachments/assets/02c9f16f-233c-4d3a-af69-cce9e7371527)





