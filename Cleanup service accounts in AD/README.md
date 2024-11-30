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
