# Summary

When running `adprep /domainprep` on Windows Server 2016, an unintended access control entry (ACE) is added to the security descriptor of the Domain Naming Context for the Enterprise Key Admins group (SID = -527). This ACE incorrectly grants full access to the naming context and all its child objects. The intended ACE should provide only `ReadProperty` and `WriteProperty` permissions for the `msDS-KeyCredentialLink` attribute.
