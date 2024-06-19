# OWASP-Top-10-Proactive-Controls

## Access Control

### Design Access Control Thoroughly Upfront

After selecting a particular access control design pattern, re-engineering the access control in your application with a different pattern can be challenging and time-consuming. Thorough upfront design of access control is crucial, especially for addressing requirements such as multi-tenancy (single-instance, mulitple users) and horizontal (data-dependent; users may only be able to access data relevant to their department or role) access control.

Two options should be considered:

- Role-Based Access Control
- Attribute-Based Access Control

### Force Every Access Request to Go Through an Access Control Check

Ensure all access request are forced to go through verification.

Examples include:

- Spring Security
- ASP.NET Core Middleware
- Django Middleware
- Node.js Middleware
- ACLs
- OAuth and OpenID Connect
- API Gateways
- WebApp Firewalls

### Consolidate the access control check

Use a centralized approach to this and resuse it throughout your code, refining it along the way. 

### Deny by Default

All request should be deined by default, unless they are specifically allowed. Some examples include the following:

- App Code may throw an error or exception while processing access control requests and hsould be denied in these instances. 
- When a new account is created that user should have little no permissions configured by default.
- When a new feature is added to an app, it should always be denied access until is configured correctly and approved. 

### Principle of Least Privilege/ Just in Time, Just Enough Access

Roles and accounts within the organization or application are only used when required, and only given enough time to complete the proccess being executed. 

### Do not Hardcode Roles

Role-based programming of this nature is fragile. It is easy to create incorrect or missing role checks in code.

Hard-Coded Roles do not allow for multi-tenancy. Extreme measures like forking the code or adding checks for each customer will be required to allow role-based systems to have different rules for different customers.

Large codebases with many access control checks can make it difficult to audit or verify the overall application access control policy.

Hard coded roles can also be seen as a backdoor when discovered during audits.

### ABAC Policy Enforcement Point Example

ABAC is a more flexible and granular access control model compared to Role-Based Access Control (RBAC). Instead of assigning permissions based solely on a user's role, ABAC evaluates access requests based on various attributes related to the user, the resource, and the environment. These attributes can include:

- User Attributes: Information about the user (e.g., user role, department, security clearance).
- Resource Attributes: Information about the resource (e.g., resource type, classification level).
- Environment Attributes: Contextual information (e.g., time of day, location, IP address).
- Action Attributes: The specific action being requested (e.g., read, write, delete).

Here is an exmaple of this in Azure:

```
{
    "RoleDefinition": {
        "Name": "CustomDeleteRole",
        "Permissions": [
            {
                "Actions": ["Microsoft.Resources/subscriptions/resourceGroups/delete"]
            }
        ],
        "AssignableScopes": ["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}"]
    }
}
```

## Use Cryptogrpahy the proper way

### Protect data at rest

- Store Passwords or secrets safely
    - Applications contain numerous “secrets” that are needed for security operations. These include certificates, SQL connection passwords, third party service account credentials, passwords, SSH keys, encryption keys and more.
- Key Lifecycle
    - Protect keys from unauthorized access
    - Ensure key rotation can be handled gracefully


### Protect data in transit

- Use current cryptographic and internally approved protocols
    - i.e. tls1.2 and above
    - Ensure the use of older protocols are not supported
    - Always utilize a secure random number generator

- Instruct Clients to enforce Transport Level Encryption
    - HSTS Headers
    - Content-Security-Policy allows for automatic client-side upgrade from HTTP to HTTPS.
    - When setting cookies, always utilize the “secure” flag to prevent transmission over HTTP.

### Cryptography changes over time

Always stay up to date with changes to recommended and approved cryptography.

## Validate all Input & Handle Exceptions

