+++
title = "Security of Distributed Software - Lecture 07"
author = ["eo shiru"]
date = 2019-05-13
lastmod = 2019-06-28T13:20:12+02:00
tags = ["uni", "security-ds"]
draft = false
+++

## Management of Access Rights {#management-of-access-rights}

**Authorization** is the process of verification and access right assignment for a resource/service to a subject and is not to be confused with _Authentication_ which is the process of verificating claimed properties. Access Control is a process of access rights management and control.

**Access Matrix**<br />
![](/knowledge-database/images/access-matrix.png)

-   group- and role-based access rights management:
    -   complexity reduction by clustering users into 'role groups'
    -   inheritance relationships in rights management
    -   permissions based on roles

**Access Control Lists**<br />
![](/knowledge-database/images/access-list.png)

-   _principal_ is a user, group or process that can be authenticated
-   simply put: ACL is a set/list of resources, principals and corresponding access rights

**Access Control Models**<br />

-   Discretionary Access Control (DAC)
    -   access rights are assigned per user
    -   owner of a resource can pass his own rights
-   Mandatory Access Control (MAC)
    -   rights passing is not allowed
    -   the system alone decides on which user has access to which resources
-   Role-Based Access Control (RBAC)
    -   user could potentially be assigned multiple roles
    -   access rights are role-based


### Realization in Operating Systems {#realization-in-operating-systems}

Unix/Linux

-   data/directories are associated to an inode descriptor (contains ID of the owner, ID of the group, ACL etc)
-   assignment of rights to the file owner, group, everyone else

Windows 2000/XP/Vista/7

-   permissions/restrictions can be assigned to individual users and groups
-   security descriptors contain owner-ID, group-ID, Access Control Elements with Allow/Deny entries, logging operations
