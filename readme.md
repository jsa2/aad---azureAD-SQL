# E2E authentication using Azure AD for Azure SQL Database 

## Setup

setting | value
- | -
Tested on Azure SQL Database | `` Standard S1: 20 DTUs & Basic `` | 
| Azure AD Application Configured for API scope |``https://database.windows.net/user_impersonation`` 
| MS Docs Source 1 | [provision-azure-ad-admin-sql-database](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?tabs=azure-powershell#provision-azure-ad-admin-sql-database) 
MS Docs Source 2| [connect-query-nodejs](https://docs.microsoft.com/en-us/azure/azure-sql/database/connect-query-nodejs?tabs=windows)
app that you can use to get access tokens | I am using simple Node.JS app for this. various tutorials exist for this part if needed


## Ensure Azure AD Application has permissions for Azure SQL Database
![img](img/app.png)

## Enable Azure AD Admin on database
- This step is outlined in the [provision-azure-ad-admin-sql-database](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?tabs=azure-powershell#provision-azure-ad-admin-sql-database)

## Add principal for the database and grant access to existing database

- You can use the query editor in portal to create the user in DB
![img](img/query.png)

- Create table Persons
```sql
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
);
```

```sql
CREATE USER [Dennis.Hale@dewi.red] FROM EXTERNAL PROVIDER;
GRANT SELECT ON [dbo].[Persons] TO  [Dennis.Hale@dewi.red];
EXEC sp_table_privileges   
   @table_name = 'Persons%';  
```


## Query with Node.JS

```javascript
// Token is provided by app of your choosing (Not included in this sample)
const config = {
    server: "latestings.database.windows.net",
    authentication: {
        type: 'azure-active-directory-access-token',
        options:{
            token
        }
    },
    options: {
        database: 'userdbs2',
        encrypt: true,
        port: 1433,
        token,
    }
};

```




