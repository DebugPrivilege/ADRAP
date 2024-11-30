# Kerberoasting

Kerberoasting is an attack technique that targets the `servicePrincipalName` (SPN) attribute in Active Directory, which is used to associate service accounts with specific services. Attackers request Kerberos service tickets for accounts with SPNs, extract these tickets (encrypted with the service account's password hash), and attempt to crack them offline. If successful, they gain access to the service account, especially if the account has a weak or poorly managed password.

## Remediation

A practical first step to mitigating Kerberoastable accounts, beyond enforcing strong passwords on SPN accounts or using gMSAs, is to review all accounts with an SPN and verify whether the server referenced in the SPN is still active. A common scenario is discovering that the server where the service account was used has already been decommissioned, yet the service account remains enabled in Active Directory.

1. Before getting started, make sure you run the script on a domain-joined machine with the Active Directory PowerShell module installed.
2. We can run the `ManageSPN.ps1` script with the `-Kerberoast` parameter to list enabled user accounts that have an SPN, along with their `pwdLastSet` and `servicePrincipalName` attributes.

