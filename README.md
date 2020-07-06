
# MSSQL Server Data Sync

Hello World.!
Hereby sharing a working method to perform table syncing between 2 MS SQL database server irrespective on the editions used.


# Requirements

 1. WCOM SqlBulkSync executable.
 2. Hron: Serialization format used for sync jobs.
 3. At Source/Destination Databases:
	* Same table schema and naming.
	* Change tracking is enabled at source DB.
	* A 'sync' schema present at destination DB.


## Usage
1. Create a Job.
>SqlBulkSync.exe CREATETEMPLATE SyncJob.hron

2. Modify  SyncJob.hron with respect to the requirements.
```  
  =SourceDbConnection
     	Data Source=sourceserver;Initial Catalog=SourceDatabase;User ID=userid;Password=password
     =TargetDbConnection
     	Data Source=targetserver;Initial Catalog=DestinationDatabase;User ID=userid;Password=password
     =Tables
     	dbo.Table1
     =Tables
     	dbo.Table2
     =BatchSize
     	1000
``` 
3. Enable Change tracking at Source Database. (SSMS or T-SQL Query)

>USE SourceDatabase
	GO 
	
> ALTER DATABASE SourceDatabase 
> SET CHANGE_TRACKING = ON
> (CHANGE_RETENTION = 5 DAYS, AUTO_CLEANUP = ON); 
> GO

4. Create 'sync'  schema at Destination Database.

>   USE DestinationDatabase
>   GO
>   
>   CREATE SCHEMA 
>   sync GO

5. Execute the Sync job
>SqlBulkSync.exe PROCESS OrderTablesToAzure.hron


# Synchronization

