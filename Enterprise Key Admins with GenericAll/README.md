# Summary

When running `adprep /domainprep` on Windows Server 2016, an unintended access control entry (ACE) is added to the security descriptor of the Domain Naming Context for the Enterprise Key Admins group (SID = -527). This ACE incorrectly grants full access to the naming context and all its child objects. The intended ACE should provide only `ReadProperty` and `WriteProperty` permissions for the `msDS-KeyCredentialLink` attribute.

![image](https://github.com/user-attachments/assets/4054ecd0-d2c8-4be6-a1d5-89e973d0d717)

## Remediation

1. First, make sure you are on a domain-joined machine with temporary Enterprise Admin privileges. This level of access is necessary because, in a single forest with multiple domains, it ensures the issue is remediated across all domains within the forest.

2. Download and run the following script: https://gist.github.com/DebugPrivilege/f731e24555efeba23bcdddd1c2c368df

![image](https://github.com/user-attachments/assets/28407c93-da64-4a6f-a7d4-ffed3d906375)

3. After the script finishes running, you should see an output similar to this:

![image](https://github.com/user-attachments/assets/cb934883-facc-4d46-9c2d-97d6bb01c737)

4. Now, let’s confirm that the **GenericAll** permission has been removed from the **Enterprise Key Admins** group on the **Domain Naming Context**.

