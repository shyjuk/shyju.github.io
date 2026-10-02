---
title: "Active Directory to MSSQL"
date: 2011-05-02
categories: 
  - "technical"
tags: 
  - "active-directory"
  - "add-users"
  - "mssql"
  - "win32"
---

1. Create a database (here it is OPE), with required columns.
2.  Open a 'New Query'  window in  Microsoft SQL Server Management Studio Express and put the below SQL Code there.
3. Find and Replace(Ctrl+H) all occurance of DC=epillars and DC=local with the appropriate values from the Active Directory.
4. Execute(F5) the Query.
5. The Query will create a Linked Server ADSI(Server Objects->Linked Servers->ADSI) and a view  ADView(Databases->OPE->Views->dbo.ADView) which contains users on active directory. Then the Users table in OPE database will be updated with the active directory users details.

```sql
USE [master]
GO
sp_addlinkedserver 'ADSI', 'Active Directory Service Interfaces', 'ADSDSOObject', 'adsdatasource'
GO

USE OPE
GO

CREATE VIEW ADView AS
SELECT CAST(LOWER(SUBSTRING(master.dbo.fn_varbintohexstr(objectGUID), 3, 8) + '-' + SUBSTRING(master.dbo.fn_varbintohexstr(objectGUID), 11, 4) + '-' + SUBSTRING(master.dbo.fn_varbintohexstr(objectGUID), 15, 4) + '-' + SUBSTRING(master.dbo.fn_varbintohexstr(objectGUID), 16, 4) + '-' + RIGHT(master.dbo.fn_varbintohexstr(objectGUID), 12)) AS uniqueidentifier) AS GUID,
       sAMAccountName AS Name,
       NULL AS Password,
       cn AS FullName,
       userPrincipalName AS Email,
       CAST((SELECT GUID FROM Roles WHERE Name = 'default') AS uniqueidentifier) AS RoleGUID,
       'True' AS IsActive,
       'False' AS IsAdmin,
       '1' AS UserType,
       'False' AS ResetPwd,
       '' AS OrderNo,
       GETDATE() AS PasswordCreateTime
FROM OPENQUERY(ADSI, 'SELECT userPrincipalName, cn, sAMAccountName, objectGUID FROM ''LDAP://DC=epillars,DC=local'' WHERE objectCategory=''user'' AND NOT (MemberOf=''CN=Administrators,CN=Builtin,DC=epillars,DC=local'' OR MemberOf=''CN=Guests,CN=Builtin,DC=epillars,DC=local'' OR MemberOf=''CN=IIS_WPG,CN=Users,DC=epillars,DC=local'')')
GO

USE OPE
INSERT INTO Users (GUID, Name, FullName, Email, RoleGUID, IsActive, IsAdmin, UserType, ResetPwd, PasswordCreateTime)
SELECT a.GUID, a.Name, a.FullName, a.Email, a.RoleGUID, a.IsActive, a.IsAdmin, a.UserType, a.ResetPwd, a.PasswordCreateTime
FROM ADView AS a
WHERE NOT EXISTS (SELECT 1 FROM Users AS b WHERE b.Name = a.Name);

SELECT * FROM Users;
```
