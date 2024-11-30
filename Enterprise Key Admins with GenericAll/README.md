# Summary

When running `adprep /domainprep` on Windows Server 2016, an unintended access control entry (ACE) is added to the security descriptor of the Domain Naming Context for the Enterprise Key Admins group (SID = -527). This ACE incorrectly grants full access to the naming context and all its child objects. The intended ACE should provide only `ReadProperty` and `WriteProperty` permissions for the `msDS-KeyCredentialLink` attribute.

![image](https://github.com/user-attachments/assets/4054ecd0-d2c8-4be6-a1d5-89e973d0d717)

## Remediation

1. First, make sure you are on a domain-joined machine with temporary Enterprise Admin privileges. This level of access is necessary because, in a single forest with multiple domains, it ensures the issue is remediated across all domains within the forest.

2. Download and run the following script: https://gist.github.com/DebugPrivilege/f731e24555efeba23bcdddd1c2c368df
