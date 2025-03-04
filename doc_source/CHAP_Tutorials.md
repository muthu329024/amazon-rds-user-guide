# Amazon RDS Tutorials<a name="CHAP_Tutorials"></a>

The AWS documentation includes several tutorials that guide you through common Amazon RDS use cases\. Many of these tutorials show you how to use Amazon RDS with other AWS services\. 

**Note**  
You can find more tutorials at the [AWS Database Blog](http://aws.amazon.com/blogs/database/)\. For information about training, see [AWS Training and Certification](https://www.aws.training/)\.

## Tutorials in this guide<a name="CHAP_Tutorials.ThisGuide"></a>

The following tutorials in this guide show you how to perform common tasks with Amazon RDS:
+ [Tutorial: Create an Amazon VPC for use with a DB instance](CHAP_Tutorials.WebServerDB.CreateVPC.md)

  Learn how to include a DB instance in an Amazon virtual private cloud \(VPC\) that shares data with a web server that is running on an Amazon EC2 instance in the same VPC\.
+ [Tutorial: Create a web server and an Amazon RDS DB instance](TUT_WebAppWithRDS.md)

  Learn how to install an Apache web server with PHP and create a MySQL database\. The web server runs on an Amazon EC2 instance using Amazon Linux, and the MySQL database is a MySQL DB instance\. Both the Amazon EC2 instance and the DB instance run in an Amazon VPC\.
+ [Tutorial: Restore a DB instance from a DB snapshot](CHAP_Tutorials.RestoringFromSnapshot.md)

  Learn how to restore a DB instance from a DB snapshot\.
+ [Tutorial: Use tags to specify which DB instances to stop](USER_Tagging.md#Tagging.RDS.Autostop)

  Learn how to use tags to specify which DB instances to stop\.
+ [Tutorial: log the state of an Amazon RDS instance using EventBridge](rds-cloud-watch-events.md#log-rds-instance-state)

  Learn how to log a DB instance stage change using Amazon EventBridge and AWS Lambda\.

## Tutorials in other AWS guides<a name="CHAP_Tutorials.OtherGuides"></a>

The following tutorials in other AWS guides show you how to perform common tasks with Amazon RDS:
+ [ Tutorial: Rotating a Secret for an AWS Database](https://docs.aws.amazon.com/secretsmanager/latest/userguide/tutorials_db-rotate.html) in the *AWS Secrets Manager User Guide*

  Learn how to create a secret for an AWS database and configure the secret to rotate on a schedule\. You trigger one rotation manually, and then confirm that the new version of the secret continues to provide access\.
+ [ Tutorial: Configuring a Lambda function to access Amazon RDS in an Amazon VPC](https://docs.aws.amazon.com/lambda/latest/dg/services-rds-tutorial.html) in the *AWS Lambda Developer Guide*

  Learn how to create a Lambda function to access a database, create a table, add a few records, and retrieve the records from the table\. You also learn how to invoke the Lambda function and verify the query results\.
+ [ Tutorials and samples](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html) in the *AWS Elastic Beanstalk Developer Guide*

  Learn how to deploy applications that use Amazon RDS databases with AWS Elastic Beanstalk\.
+ [ Using Data from an Amazon RDS Database to Create an Amazon ML Datasource](https://docs.aws.amazon.com/machine-learning/latest/dg/using-amazon-rds-with-amazon-ml.html) in the *Amazon Machine Learning Developer Guide*

  Learn how to create an Amazon Machine Learning \(Amazon ML\) datasource object from data stored in a MySQL DB instance\.
+ [ Manually Enabling Access to an Amazon RDS Instance in a VPC](https://docs.aws.amazon.com/quicksight/latest/user/rds-vpc-access.html) in the *Amazon QuickSight User Guide*

  Learn how to enable Amazon QuickSight access to an Amazon RDS DB instance in a VPC\.