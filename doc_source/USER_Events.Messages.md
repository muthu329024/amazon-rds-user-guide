# Amazon RDS event categories and event messages<a name="USER_Events.Messages"></a>

 Amazon RDS generates a significant number of events in categories that you can subscribe to using the Amazon RDS Console, AWS CLI, or the API\. Each category applies to a source type\.

**Topics**
+ [DB instance events](#USER_Events.Messages.instance)
+ [DB parameter group events](#USER_Events.Messages.parameter-group)
+ [DB security group events](#USER_Events.Messages.security-group)
+ [DB snapshot events](#USER_Events.Messages.snapshot)

## DB instance events<a name="USER_Events.Messages.instance"></a>

The following table shows the event category and a list of events when a DB instance is the source type\.


|  Category  | Amazon RDS event ID |  Description  | 
| --- | --- | --- | 
|  availability  | RDS\-EVENT\-0006 |  The DB instance restarted\.  | 
|  availability  | RDS\-EVENT\-0004 |  DB instance shutdown\.  | 
|  availability  | RDS\-EVENT\-0022 |  An error has occurred while restarting MySQL or MariaDB\.  | 
|  backup  | RDS\-EVENT\-0001 |  Backing up DB instance\.  | 
|  backup  | RDS\-EVENT\-0002 |  Finished DB Instance backup\.  | 
|  backup  | RDS\-EVENT\-0075 |  RDS finished creating a user\-initiated snapshot\.  | 
|  backup  | RDS\-EVENT\-0086 |  RDS was unable to associate the option group with the database instance\. Confirm that the option group is supported on your DB instance class and configuration\. For more information see [Working with option groups](USER_WorkingWithOptionGroups.md)\.  | 
|  configuration change  | RDS\-EVENT\-0009 |  The DB instance has been added to a security group\.  | 
|  configuration change  | RDS\-EVENT\-0024 |  The DB instance is being converted to a Multi\-AZ DB instance\.  | 
|  configuration change  | RDS\-EVENT\-0030 |  The DB instance is being converted to a Single\-AZ DB instance\.  | 
|  configuration change  | RDS\-EVENT\-0012 |  Applying modification to database instance class\.   | 
|  configuration change  | RDS\-EVENT\-0018 |  The current storage settings for this DB instance are being changed\.  | 
|  configuration change  | RDS\-EVENT\-0011 |  A parameter group for this DB instance has changed\.  | 
|  configuration change  | RDS\-EVENT\-0092 |  A parameter group for this DB instance has finished updating\.  | 
|  configuration change  | RDS\-EVENT\-0028 |  Automatic backups for this DB instance have been disabled\.  | 
|  configuration change  | RDS\-EVENT\-0032 |  Automatic backups for this DB instance have been enabled\.  | 
|  configuration change  | RDS\-EVENT\-0033 |  There are \[count\] users that match the master user name\. Users not tied to a specific host have been reset\.  | 
|  configuration change  | RDS\-EVENT\-0025 |  The DB instance has been converted to a Multi\-AZ DB instance\.  | 
|  configuration change  | RDS\-EVENT\-0029 |  The DB instance has been converted to a Single\-AZ DB instance\.  | 
|  configuration change  | RDS\-EVENT\-0014 |  The DB instance class for this DB instance has changed\.  | 
|  configuration change  | RDS\-EVENT\-0017 |  The storage settings for this DB instance have changed\.  | 
|  configuration change  | RDS\-EVENT\-0010 |  The DB instance has been removed from a security group\.  | 
|  configuration change  | RDS\-EVENT\-0016 |  The master password for the DB instance has been reset\.  | 
|  configuration change  | RDS\-EVENT\-0067 |  An attempt to reset the master password for the DB instance has failed\.  | 
|  configuration change  | RDS\-EVENT\-0078 |  The Enhanced Monitoring configuration has been changed\.  | 
|  creation  | RDS\-EVENT\-0005 |  DB instance created\.  | 
|  deletion  | RDS\-EVENT\-0003 |  The DB instance has been deleted\.  | 
|  failover  | RDS\-EVENT\-0034 |  Amazon RDS is not attempting a requested failover because a failover recently occurred on the DB instance\.  | 
|  failover  | RDS\-EVENT\-0013 |  A Multi\-AZ failover that resulted in the promotion of a standby instance has started\.  | 
|  failover  | RDS\-EVENT\-0015 |  A Multi\-AZ failover that resulted in the promotion of a standby instance is complete\. It may take several minutes for the DNS to transfer to the new primary DB instance\.  | 
|  failover  | RDS\-EVENT\-0065 |  The instance has recovered from a partial failover\.  | 
|  failover  | RDS\-EVENT\-0049 | A Multi\-AZ failover has completed\. | 
|  failover  | RDS\-EVENT\-0050 |  A Multi\-AZ activation has started after a successful instance recovery\.  | 
|  failover  | RDS\-EVENT\-0051 |  A Multi\-AZ activation is complete\. Your database should be accessible now\.  | 
|  failure  | RDS\-EVENT\-0031 |  The DB instance has failed due to an incompatible configuration or an underlying storage issue\. Begin a point\-in\-time\-restore for the DB instance\.  | 
|  failure  | RDS\-EVENT\-0036 |  The DB instance is in an incompatible network\. Some of the specified subnet IDs are invalid or do not exist\.  | 
|  failure  | RDS\-EVENT\-0035 |  The DB instance has invalid parameters\. For example, if the DB instance could not start because a memory\-related parameter is set too high for this instance class, the customer action would be to modify the memory parameter and reboot the DB instance\.  | 
|  failure  | RDS\-EVENT\-0058 |  Error while creating Statspack user account PERFSTAT\. Please drop the account before adding the Statspack option\.  | 
|  failure  | RDS\-EVENT\-0079 |  Enhanced Monitoring cannot be enabled without the enhanced monitoring IAM role\. For information on creating the enhanced monitoring IAM role, see [To create an IAM role for Amazon RDS enhanced monitoring](USER_Monitoring.OS.Enabling.md#USER_Monitoring.OS.IAMRole)\.  | 
|  failure  | RDS\-EVENT\-0080 |  Enhanced Monitoring was disabled due to an error making the configuration change\. It is likely that the enhanced monitoring IAM role is configured incorrectly\. For information on creating the enhanced monitoring IAM role, see [To create an IAM role for Amazon RDS enhanced monitoring](USER_Monitoring.OS.Enabling.md#USER_Monitoring.OS.IAMRole)\.  | 
|  failure  | RDS\-EVENT\-0081 |  The IAM role that you use to access your Amazon S3 bucket for SQL Server native backup and restore is configured incorrectly\. For more information, see [Setting up for native backup and restore](SQLServer.Procedural.Importing.md#SQLServer.Procedural.Importing.Native.Enabling)\.  | 
|  failure  | RDS\-EVENT\-0188 |  Amazon RDS was unable to upgrade a MySQL DB instance from version 5\.7 to version 8\.0 because of incompatibilities related to the data dictionary\. The DB instance was rolled back to MySQL version 5\.7\. For more information, see [Rollback after failure to upgrade from MySQL 5\.7 to 8\.0](USER_UpgradeDBInstance.MySQL.md#USER_UpgradeDBInstance.MySQL.Major.RollbackAfterFailure)\.  | 
|  low storage  | RDS\-EVENT\-0089 |  The DB instance has consumed more than 90% of its allocated storage\. You can monitor the storage space for a DB instance using the **Free Storage Space** metric\.  | 
|  low storage  | RDS\-EVENT\-0007 |  The allocated storage for the DB instance has been consumed\. To resolve this issue, allocate additional storage for the DB instance\. For more information, see the [RDS FAQ](https://aws.amazon.com/rds/faqs/#20)\. You can monitor the storage space for a DB instance using the **Free Storage Space** metric\.  | 
|  maintenance  | RDS\-EVENT\-0026 |  Offline maintenance of the DB instance is taking place\. The DB instance is currently unavailable\.  | 
|  maintenance  | RDS\-EVENT\-0027 |  Offline maintenance of the DB instance is complete\. The DB instance is now available\.  | 
| maintenance | RDS\-EVENT\-0047 | Patching of the DB instance has completed\. | 
|  maintenance  | RDS\-EVENT\-0155 |  The DB instance has a DB engine minor version upgrade available\.  | 
|  notification  | RDS\-EVENT\-0044 | Operator\-issued notification\. For more information, see the event message\. | 
|  notification  | RDS\-EVENT\-0048 | Patching of the DB instance has been delayed\. | 
|  notification  | RDS\-EVENT\-0054 | The MySQL storage engine you are using is not InnoDB, which is the recommended MySQL storage engine for Amazon RDS\. For information about MySQL storage engines, see [ Supported storage engines for MySQL on Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Concepts.Storage.html)\. | 
|  notification  | RDS\-EVENT\-0055 |  The number of tables you have for your DB instance exceeds the recommended best practices for Amazon RDS\. Please reduce the number of tables on your DB instance\.    For information about recommended best practices, see [Amazon RDS basic operational guidelines](CHAP_BestPractices.md#CHAP_BestPractices.DiskPerformance)\.   | 
|  notification  | RDS\-EVENT\-0056 |  The number of databases you have for your DB instance exceeds the recommended best practices for Amazon RDS\. Please reduce the number of databases on your DB instance\.    For information about recommended best practices, see [Amazon RDS basic operational guidelines](CHAP_BestPractices.md#CHAP_BestPractices.DiskPerformance)\.   | 
|  notification  | RDS\-EVENT\-0064 | The TDE key has been rotated\. For information about recommended best practices, see [Amazon RDS basic operational guidelines](CHAP_BestPractices.md#CHAP_BestPractices.DiskPerformance)\.  | 
|  notification  | RDS\-EVENT\-0084 |  You attempted to convert a DB instance to Multi\-AZ, but it contains in\-memory file groups that are not supported for Multi\-AZ\. For more information, see [Multi\-AZ deployments for Microsoft SQL Server](USER_SQLServerMultiAZ.md)\.   | 
|  notification  | RDS\-EVENT\-0087 |  The DB instance has been stopped\.   | 
|  notification  | RDS\-EVENT\-0088 |  The DB instance has been started\.  | 
|  notification  | RDS\-EVENT\-0154 |  The DB instance is being started due to it exceeding the maximum allowed time being stopped\.  | 
|  notification  | RDS\-EVENT\-0157 |  RDS can't modify the DB instance class because the target instance class can't support the number of databases that exist on the source DB instance\. The error message appears as: "The instance has *N* databases, but after conversion it would only support *N*"\. For more information, see [Limits for Microsoft SQL Server DB instances](CHAP_SQLServer.md#SQLServer.Concepts.General.FeatureSupport.Limits)\.  | 
|  notification  | RDS\-EVENT\-0158 |  DB instance is in a state that can't be upgraded\.  | 
|  notification  | RDS\-EVENT\-0189 |  The gp2 burst balance credits for the DB instance are running low\. This can lead to a noticeable performance decline in the DB\. To resolve this issue, reduce IOPS usage or modify your storage settings to enable more performance\. For more information, see [I/O credits and burst performance](CHAP_Storage.md#CHAP_Storage.IO.Credits)\.  | 
|  read replica  | RDS\-EVENT\-0045 |  An error has occurred in the read replication process\. For more information, see the event message\.  In addition, see the troubleshooting section for read replicas for your DB engine\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Events.Messages.html)   | 
|  read replica  | RDS\-EVENT\-0046 | The read replica has resumed replication\. This message appears when you first create a read replica, or as a monitoring message confirming that replication is functioning properly\. If this message follows an RDS\-EVENT\-0045 notification, then replication has resumed following an error or after replication was stopped\. | 
|  read replica  | RDS\-EVENT\-0057 |  Replication on the read replica was terminated\.  | 
|  read replica  | RDS\-EVENT\-0062 |  Replication on the read replica was manually stopped\.  | 
|  read replica  | RDS\-EVENT\-0063 |  Replication on the read replica was reset\.  | 
|  recovery  | RDS\-EVENT\-0020 |  Recovery of the DB instance has started\. Recovery time will vary with the amount of data to be recovered\.  | 
|  recovery  | RDS\-EVENT\-0021 |  Recovery of the DB instance is complete\.  | 
|  recovery  | RDS\-EVENT\-0023 |  A manual backup has been requested but Amazon RDS is currently in the process of creating a DB snapshot\. Submit the request again after Amazon RDS has completed the DB snapshot\.  | 
|  recovery  | RDS\-EVENT\-0052 |  Recovery of the Multi\-AZ instance has started\. Recovery time will vary with the amount of data to be recovered\.  | 
|  recovery  | RDS\-EVENT\-0053 |  Recovery of the Multi\-AZ instance is complete\.  | 
|  recovery  | RDS\-EVENT\-0066 |  The SQL Server DB instance is re\-establishing its mirror\. Performance will be degraded until the mirror is reestablished\. A database was found with non\-FULL recovery model\. The recovery model was changed back to FULL and mirroring recovery was started\. \(<dbname>: <recovery model found>\[,\.\.\.\]\)"  | 
|  restoration  | RDS\-EVENT\-0008 |  The DB instance has been restored from a DB snapshot\.  | 
|  restoration  | RDS\-EVENT\-0019 |  The DB instance has been restored from a point\-in\-time backup\.  | 
|  security  | RDS\-EVENT\-0068 |  RDS is decrypting the CloudHSM partition password to make updates to the instance\. For more information see [Oracle Database Transparent Data Encryption \(TDE\) with AWS CloudHSM](https://docs.aws.amazon.com/cloudhsm/latest/userguide/oracle-tde.html) in the *AWS CloudHSM User Guide*\.  | 

## DB parameter group events<a name="USER_Events.Messages.parameter-group"></a>

The following table shows the event category and a list of events when a DB parameter group is the source type\.


|  Category  | RDS event ID |  Description  | 
| --- | --- | --- | 
|  configuration change  | RDS\-EVENT\-0037 |  The parameter group was modified\.  | 

## DB security group events<a name="USER_Events.Messages.security-group"></a>

The following table shows the event category and a list of events when a DB security group is the source type\.


|  Category  | RDS event ID |  Description  | 
| --- | --- | --- | 
|  configuration change  | RDS\-EVENT\-0038 |  The security group has been modified\.  | 
|  failure  | RDS\-EVENT\-0039 |  The security group owned by \[user\] does not exist; authorization for the security group has been revoked\.  | 

## DB snapshot events<a name="USER_Events.Messages.snapshot"></a>

The following table shows the event category and a list of events when a DB snapshot is the source type\.


|  Category  | RDS event ID |  Description  | 
| --- | --- | --- | 
|  creation  | RDS\-EVENT\-0040 |  A manual DB snapshot is being created\.  | 
|  creation  | RDS\-EVENT\-0042 |  A manual DB snapshot has been created\.  | 
| creation | RDS\-EVENT\-0090 | An automated DB snapshot is being created\. | 
| creation | RDS\-EVENT\-0091 | An automated DB snapshot has been created\. | 
|  deletion  | RDS\-EVENT\-0041 |  A DB snapshot has been deleted\.  | 
|  notification  | RDS\-EVENT\-0059 |  Started copy of snapshot \[DB snapshot name\] from region \[region name\]\.  | 
|  notification  | RDS\-EVENT\-0060 |  Finished copy of snapshot \[DB snapshot name\] from region \[region name\] in \[time\] minutes\.  | 
|  notification  | RDS\-EVENT\-0061 |  Canceled snapshot copy request of \[DB snapshot name\] from region %\[region name\]\.  | 
| notification | RDS\-EVENT\-0159 |  DB snapshot export task failed\.  | 
| notification | RDS\-EVENT\-0160 |  DB snapshot export task canceled\.  | 
| notification | RDS\-EVENT\-0161 |  DB snapshot export task completed\.  | 
|  restoration  | RDS\-EVENT\-0043 |  A DB instance is being restored from a DB snapshot\.  | 