# Summary

This PowerShell script helps administrators find and manage Tier 0 groups and users in Active Directory. These groups and users have high-level privileges, and if compromised, could impact the entire domain. The script comes with a predefined list of AD groups that are treated as Tier 0 by default. You should carefully review these built-in AD groups to identify any accounts that can be removed.

## Remediation

1. Before getting started, make sure you run the script on a domain-joined machine with the Active Directory PowerShell module installed.
2. Run the PowerShell script: https://gist.github.com/DebugPrivilege/74acc540b7f0e2fb2a929edbf1ee0a7d

```
.\Get-T0Groups.ps1
```

![image](https://github.com/user-attachments/assets/d4d21813-4f4d-40b9-b531-25749f2be44c)

## Best Practices

For every account, ask:
- Why is it a member of this group?
- What tasks does it need to perform?
- Can those tasks be performed with lower privileges through delegation?

