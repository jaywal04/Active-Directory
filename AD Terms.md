
| Term                         | Meaning                                                                                                                                   |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Active Directory (AD)**    | The centralized directory and identity service for Windows networks. Stores accounts and enforces who may log in and what they may reach. |
| **Authentication**           | Proving *who you are*. The logon step.                                                                                                    |
| **Authorization**            | Deciding *what you are allowed to do* once authenticated. Permissions.                                                                    |
| **Domain Controller (DC)**   | The Windows Server that hosts the AD database, answers logon requests, and runs directory services.                                       |
| **AD database**              | The store itself, held on each DC as `NTDS.dit`.                                                                                          |
| **Directory services**       | The lookup, replication, and authentication services a DC provides.                                                                       |
| **Object**                   | Any single item AD tracks. Everything in AD is an object.                                                                                 |
| **User**                     | An object representing one person's account.                                                                                              |
| **Device / Computer**        | An object representing a machine joined to the domain. Officially a *computer object*.                                                    |
| **Shared folder**            | A network share published into the directory so users can find it.                                                                        |
| **Organizational Unit (OU)** | A folder inside a domain used to organize objects and to target Group Policy. Not a security boundary.                                    |
| **Sub-OU**                   | An OU nested inside another OU. Group Policy inherits down the chain.                                                                     |
| **Domain**                   | The logical boundary of users and computers sharing one database and one security policy. Where password policy and authentication live.  |
| **Security policy**          | The rules (password length, lockout, etc.) applied across a domain.                                                                       |


---
## 2. Structure: Domain, Tree, Forest

| Term                   | Meaning                                                                                                                                                                                 |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Domain**             | One login database, one password policy, its own DC(s).                                                                                                                                 |
| **Tree**               | A parent domain plus child domains whose DNS names nest under it. `med.college.local` is a child of `college.local`. Called a *contiguous namespace*.                                   |
| **Forest**             | The outermost container. One or more trees that all automatically trust each other and share one schema and one Global Catalog. Named after its first domain, the *forest root domain*. |
| **Namespace**          | The DNS naming space a tree occupies. Contiguous within a tree, separate between trees.                                                                                                 |
| **Trust**              | The relationship that lets one domain accept accounts from another. Automatic and two-way between all domains in a forest.                                                              |
| **Domain name / FQDN** | The DNS name of a domain, e.g. `domain.local`.                                                                                                                                          |
| **NetBIOS name**       | The short legacy name of the domain, e.g. `DOMAIN`. Shown as `DOMAIN\username` at logon.                                                                                                |


---
## 4. Groups

| Term                          | Meaning                                                                                                 |
| ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Group**                     | A container of accounts, used either to grant access or to send mail.                                   |
| **Group scope**               | How far across the forest a group reaches. Universal, Global, or Domain Local.                          |
| **Universal**                 | Members from any domain in the forest; usable for permissions anywhere in the forest.                   |
| **Global**                    | Members from its own domain only; usable for permissions anywhere in the forest.                        |
| **Domain Local**              | Members from any domain in the forest; usable for permissions only within its own domain.               |
| **Group type**                | Whether the group grants access (Security) or only sends mail (Distribution).                           |
| **Security group**            | Can be placed on an access control list to grant permissions.                                           |
| **Built-in vs custom groups** | Built-in groups ship with AD (Domain Admins, Users); custom groups are ones you create.                 |
| **Distribution group**        | Email only. Cannot be used to grant any access.                                                         |
| **Distribution list (DL)**    | The mailing list produced by a distribution group, e.g. one address fanning out to 5,000 inboxes.       |
| **Exchange**                  | The Microsoft mail server that consumes distribution groups.                                            |
| **Permission / Resource**     | The access right granted, and the file, folder, or service it applies to.                               |
| **Password policy**           | Complexity, length, and age rules. Set at the **domain** level via Default Domain Policy, not per user. |

### Group scope at a glance

| Scope            | Who can be a member  | Where it can be given permissions |
| ---------------- | -------------------- | --------------------------------- |
| **Domain Local** | Anyone in the forest | Its own domain only               |
| **Global**       | Its own domain only  | Anywhere in the forest            |
| **Universal**    | Anyone in the forest | Anywhere in the forest            |


---
## 5. Terms Not in the Source Note


| Term | Meaning |
| --- | --- |
| **Schema** | The forest-wide definition of what object types and attributes can exist. Extending it affects every domain in the forest. |
| **Global Catalog (GC)** | A partial, searchable index of every object in the forest, held on designated DCs. What makes forest-wide lookup and Universal group membership work. |
| **AGDLP** | The standard permissions pattern: **A**ccounts into a **G**lobal group, into a **D**omain **L**ocal group, which receives the **P**ermission. Lets you manage access by group membership instead of touching folder ACLs. |
| **Enterprise Admins** | The forest-wide administrative group. The escalation target that makes the forest the security boundary. |
| **Fine-Grained Password Policy (PSO)** | A Password Settings Object, the only supported way to apply a different password policy to specific users or groups within a domain. |




Physical Site:
Intra-site Replication: DC replicate data automatically
Intersite Replication: between site replication via SMTP
![[AD Terms-1789833759498.webp|757]]

