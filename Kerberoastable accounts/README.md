# Kerberoasting

Kerberoasting is an attack technique that targets the servicePrincipalName (SPN) attribute in Active Directory, which is used to associate service accounts with specific services. Attackers request Kerberos service tickets for accounts with SPNs, extract these tickets (encrypted with the service account's password hash), and attempt to crack them offline. If successful, they gain access to the service account, especially if the account has a weak or poorly managed password.
