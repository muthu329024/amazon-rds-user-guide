# Rebooting a DB instance<a name="USER_RebootInstance"></a>

You might need to reboot your DB instance, usually for maintenance reasons\. For example, if you make certain modifications, or if you change the DB parameter group associated with the DB instance, you must reboot the instance for the changes to take effect\. 

**Note**  
If a DB instance isn't using the latest changes to its associated DB parameter group, the AWS Management Console shows the DB parameter group with a status of **pending\-reboot**\. The **pending\-reboot** parameter groups status doesn't result in an automatic reboot during the next maintenance window\. To apply the latest parameter changes to that DB instance, manually reboot the DB instance\. For more information about parameter groups, see [Working with DB parameter groups](USER_WorkingWithParamGroups.md)\.

Rebooting a DB instance restarts the database engine service\. Rebooting a DB instance results in a momentary outage, during which the DB instance status is set to *rebooting*\. 

 If the Amazon RDS instance is configured for Multi\-AZ, you can perform the reboot with a failover\. An Amazon RDS event is created when the reboot is completed\. If your DB instance is a Multi\-AZ deployment, you can force a failover from one Availability Zone \(AZ\) to another when you reboot\. When you force a failover of your DB instance, Amazon RDS automatically switches to a standby replica in another Availability Zone, and updates the DNS record for the DB instance to point to the standby DB instance\. As a result, you need to clean up and re\-establish any existing connections to your DB instance\. Rebooting with failover is beneficial when you want to simulate a failure of a DB instance for testing, or restore operations to the original AZ after a failover occurs\. For more information, see [High availability \(Multi\-AZ\) for Amazon RDS](Concepts.MultiAZ.md)\. 

**Note**  
When you force a failover from one Availability Zone to another when you reboot, the Availability Zone change might not be reflected in the AWS Management Console, and in calls to the AWS CLI and RDS API, for several minutes\.

You can't reboot your DB instance if it is not in the available state\. Your database can be unavailable for several reasons, such as an in\-progress backup, a previously requested modification, or a maintenance\-window action\. 

The time required to reboot your DB instance depends on the crash recovery process, database activity at the time of reboot, and the behavior of your specific DB engine\. To improve the reboot time, we recommend that you reduce database activity as much as possible during the reboot process\. Reducing database activity reduces rollback activity for in\-transit transactions\. 

For a DB instance with read replicas, you can reboot the source DB instance and its read replicas independently\. After a reboot completes, replication resumes automatically\.

## Console<a name="USER_RebootInstance.Console"></a>

**To reboot a DB instance**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Databases**, and then choose the DB instance that you want to reboot\. 

1. For **Actions**, choose **Reboot**\. 

   The **Reboot DB Instance** page appears\.

1. \(Optional\) Choose **Reboot with failover?** to force a failover from one AZ to another\. 

1. Choose **Reboot** to reboot your DB instance\. 

   Alternatively, choose **Cancel**\. 

## AWS CLI<a name="USER_RebootInstance.CLI"></a>

To reboot a DB instance by using the AWS CLI, call the [https://docs.aws.amazon.com/cli/latest/reference/rds/reboot-db-instance.html](https://docs.aws.amazon.com/cli/latest/reference/rds/reboot-db-instance.html) command\. 

**Example Simple reboot**  
For Linux, macOS, or Unix:  

```
aws rds reboot-db-instance \
    --db-instance-identifier mydbinstance
```
For Windows:  

```
aws rds reboot-db-instance ^
    --db-instance-identifier mydbinstance
```

**Example Reboot with failover**  
To force a failover from one AZ to the other, use the `--force-failover` parameter\.   
For Linux, macOS, or Unix:  

```
aws rds reboot-db-instance \
    --db-instance-identifier mydbinstance \
    --force-failover
```
For Windows:  

```
aws rds reboot-db-instance ^
    --db-instance-identifier mydbinstance ^
    --force-failover
```

## RDS API<a name="USER_RebootInstance.API"></a>

To reboot a DB instance by using the Amazon RDS API, call the [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html) operation\. 