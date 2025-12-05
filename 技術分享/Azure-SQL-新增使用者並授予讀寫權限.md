[[_TOC_]]


# Azure SQL 帳號、角色 授權



```
USE master
CREATE LOGIN TestAccount WITH PASSWORD='@pa1xc*xMfkrv^~y_nqy'
CREATE USER TestApp FOR LOGIN TestAccount
GO

USE USOrder
CREATE USER TestApp FOR LOGIN TestAccount
EXEC sp_addrolemember 'db_datareader', TestApp;
EXEC sp_addrolemember 'db_datawriter', TestApp;
GO
```


- LOGIN 是登入帳號，包含密碼、原則、過期、禁用…ETC.
單純登入資料庫伺服器並無法存取資料庫
- USER 是使用者，包含角色、權限…ETC.
關聯登入帳號到資料庫的使用者後，才能夠以對應資料庫的權限來存取，
每個資料庫權限不同，須個別設定使用者權限
首先登入帳號/密碼跟使用者是分開的

登入 => 使用者 => 權限 => 資料庫

這邊先建立登入帳號與密碼 (這部分是存在 master)

帳號：NewLoginAccount
密碼：donthackmeplz


```
USE master
CREATE LOGIN [NewLoginAccount] WITH PASSWORD='donthackmeplz'
GO
```

<IMG  src="https://dotblogsfile.blob.core.windows.net/user/%E5%B0%8F%E5%B0%8F%E6%9C%B1/169980c2-8359-4a4c-ba1b-606bb9fe53ef/1682410424.png.png"/>



Login
然後ssms登入之後會到master進行一些查詢
如果沒幫這個登入建立 master 的使用者，到時候 ssms 雖然可以登入但重整會跳錯誤
所以接著用剛剛的新登入帳號來創建 master 的使用者

帳號：NewLoginAccount
使用者名稱：NewUserName

```
USE master
CREATE USER [NewUserName] FOR LOGIN [NewLoginAccount] 
GO
```
<IMG  src="https://dotblogsfile.blob.core.windows.net/user/%E5%B0%8F%E5%B0%8F%E6%9C%B1/169980c2-8359-4a4c-ba1b-606bb9fe53ef/1682410480.png.png"/>


User
接著切換到你真正要存取的資料庫
再建一次使用者


```
USE MyAppDb
CREATE USER [NewUserName] FOR LOGIN [NewLoginAccount] 
GO
```

到這邊雖然新帳號可以登入也看得到這個DB
但是會發現裡面資料表都看不到
這是因為沒有讀取權限
所以最後我們需要針對該使用者進行授權 (基於角色)


```
USE MyAppDb
EXEC sp_addrolemember 'db_datareader', [NewUserName];
GO
```


如果同時需要增刪修，就一併授予寫入權限


```
USE MyAppDb
EXEC sp_addrolemember 'db_datawriter', [NewUserName];
GO
```

到這邊一般應用程式應該差不多使用這帳號來設定連線字串就可以了
開發時如果需要 db migrations 進行資料表的增刪修
則建議使用另一個較高權限的帳號來進行操作

<IMG  src="https://dotblogsfile.blob.core.windows.net/user/jakeuj/169980c2-8359-4a4c-ba1b-606bb9fe53ef/1632377410.png"/>

參考
- [在Azure SQL上新增使用者](http://andy51002000.blogspot.com/2017/12/azure-sql.html)



<hr/>

# 從目前資料庫中的 帳號移除角色。

```
EXEC sp_droprolemember 'db_owner',  'TestUer' ;
GO
```

```
ALTER ROLE db_datawriter
	ADD MEMBER TestUer;  
GO
```


# 查詢遮罩的資料行

```
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function
FROM sys.masked_columns AS c
JOIN sys.tables AS tbl
    ON c.[object_id] = tbl.[object_id]
WHERE is_masked = 1;
```

# 帳號角色清單

```
SELECT    roles.principal_id AS RolePrincipalID
    ,    roles.name AS RolePrincipalName
    ,    database_role_members.member_principal_id    AS MemberPrincipalID
    ,    members.name AS MemberPrincipalName
FROM sys.database_role_members AS database_role_members  
JOIN sys.database_principals AS roles  
    ON database_role_members.role_principal_id = roles.principal_id  
JOIN sys.database_principals AS members  
    ON database_role_members.member_principal_id = members.principal_id;  
GO
```


<hr/>

# 建立動態資料遮罩
下列範例將建立有三種不同動態資料遮罩類型的資料表。 範例會填入資料表，並選擇顯示結果。


```
-- schema to contain user tables
CREATE SCHEMA Data;
GO
```

```
-- table with masked columns
CREATE TABLE Data.Membership (
    MemberID INT IDENTITY(1, 1) NOT NULL PRIMARY KEY CLUSTERED,
    FirstName VARCHAR(100) MASKED WITH (FUNCTION = 'partial(1, "xxxxx", 1)') NULL,
    LastName VARCHAR(100) NOT NULL,
    Phone VARCHAR(12) MASKED WITH (FUNCTION = 'default()') NULL,
    Email VARCHAR(100) MASKED WITH (FUNCTION = 'email()') NOT NULL,
    DiscountCode SMALLINT MASKED WITH (FUNCTION = 'random(1, 100)') NULL
);
```

```
-- inserting sample data
INSERT INTO Data.Membership (FirstName, LastName, Phone, Email, DiscountCode)
VALUES
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com', 10),
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co', 5),
('Shakti', 'Menon', '555.123.4570', 'SMenon@contoso.net', 50),
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net', 40);
GO
```



## 授與權限以檢視未遮蔽的資料
授與 UNMASK 權限可讓 TestUer 看見未遮罩的資料。

```
GRANT UNMASK TO TestUer;

EXECUTE AS USER = 'MaskingTestUser';

SELECT * FROM [Movie]

REVERT;
```

  
## Removing the UNMASK permission
`REVOKE UNMASK TO TestUer;`


## 在資料庫中建立不同的使用者：

```
CREATE USER ServiceAttendant WITHOUT LOGIN;
GO
CREATE USER ServiceLead WITHOUT LOGIN;
GO
CREATE USER ServiceManager WITHOUT LOGIN;
GO
CREATE USER ServiceHead WITHOUT LOGIN;
GO
```


## 授與讀取權限給資料庫中的使用者：

```
ALTER ROLE db_datareader ADD MEMBER ServiceAttendant;
ALTER ROLE db_datareader ADD MEMBER ServiceLead;
ALTER ROLE db_datareader ADD MEMBER ServiceManager;
ALTER ROLE db_datareader ADD MEMBER ServiceHead;
```


## 授與不同的 UNMASK 權限給使用者：
- Grant column level UNMASK permission to ServiceAttendant
`GRANT UNMASK ON Data.Membership(FirstName) TO ServiceAttendant;`

- Grant table level UNMASK permission to ServiceLead
`GRANT UNMASK ON Data.Membership TO ServiceLead;`

- Grant schema level UNMASK permission to ServiceManager
```
GRANT UNMASK ON SCHEMA::Data TO ServiceManager;
GRANT UNMASK ON SCHEMA::Service TO ServiceManager;
```

- Grant database level UNMASK permission to ServiceHead;
`GRANT UNMASK TO ServiceHead;`






# 授權遮罩 to 角色


## 建立角色
```
CREATE ROLE [UnMasker] AUTHORIZATION [dbo]
GO
```

## 授權帳號角色
```
ALTER ROLE [UnMasker] ADD MEMBER TestUer
GO
```


## 授權遮罩 by 角色
Grant the unmask permission to the role (Grant table level UNMASK permission)

`GRANT UNMASK ON Data.Membership TO [UnMasker];`

## 移除遮罩 by 角色

REVOKE UNMASK ON Data.Membership FROM [UnMasker];
``




參考
https://learn.microsoft.com/zh-tw/sql/relational-databases/security/dynamic-data-masking?view=sql-server-ver16



