# Working with DB parameter groups<a name="USER_WorkingWithParamGroups"></a>

 You manage your DB engine configuration by associating your DB instances with parameter groups\. Amazon RDS defines parameter groups with default settings that apply to newly created DB instances\. 

**Important**  
You can define your own parameter groups with customized settings\. Then you can modify your DB instances to use your own parameter groups\.  
For information about modifying a DB instance, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\.

 A *DB parameter group* acts as a container for engine configuration values that are applied to one or more DB instances\. 

 If you create a DB instance without specifying a DB parameter group, the DB instance uses a default DB parameter group\. Each default DB parameter group contains database engine defaults and Amazon RDS system defaults based on the engine, compute class, and allocated storage of the instance\. You can't modify the parameter settings of a default parameter group\. Instead, you create your own parameter group where you choose your own parameter settings\. Not all DB engine parameters can be changed in a parameter group that you create\. 

 If you want to use your own parameter group, you create a new parameter group and modify the parameters that you want to\. You then modify your DB instance to use the new parameter group\. If you update parameters within a DB parameter group, the changes apply to all DB instances that are associated with that parameter group\. 

 You can copy an existing DB parameter group with the AWS CLI [copy\-db\-parameter\-group](https://docs.aws.amazon.com/cli/latest/reference/rds/copy-db-parameter-group.html) command\. Copying a parameter group can be convenient when you want to include most of an existing DB parameter group's custom parameters and values in a new DB parameter group\. 

Here are some important points about working with parameters in a DB parameter group:
+ When you change a dynamic parameter and save the DB parameter group, the change is applied immediately regardless of the **Apply Immediately** setting\. When you change a static parameter and save the DB parameter group, the parameter change takes effect after you manually reboot the DB instance\. You can reboot a DB instance using the RDS console, by calling the `reboot-db-instance` CLI command, or by calling the `RebootDbInstance` API operation\. The requirement to reboot the associated DB instance after a static parameter change helps mitigate the risk of a parameter misconfiguration affecting an API call, such as calling `ModifyDBInstance` to change DB instance class or scale storage\.

  If a DB instance isn't using the latest changes to its associated DB parameter group, the AWS Management Console shows the DB parameter group with a status of **pending\-reboot**\. The **pending\-reboot** parameter groups status doesn't result in an automatic reboot during the next maintenance window\. To apply the latest parameter changes to that DB instance, manually reboot the DB instance\.
+ When you change the DB parameter group associated with a DB instance, you must manually reboot the instance before the DB instance can use the new DB parameter group\. For more information about changing the DB parameter group, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\.
+ You can specify the value for a DB parameter as an integer or as an integer expression built from formulas, variables, functions, and operators\. Functions can include a mathematical log expression\. For more information, see [Specifying DB parameters](#USER_ParamValuesRef)\.
+ Set any parameters that relate to the character set or collation of your database in your parameter group before creating the DB instance and before you create a database in your DB instance\. This ensures that the default database and new databases in your DB instance use the character set and collation values that you specify\. If you change character set or collation parameters for your DB instance, the parameter changes are not applied to existing databases\.

  You can change character set or collation values for an existing database using the `ALTER DATABASE` command, for example:

  ```
  ALTER DATABASE database_name CHARACTER SET character_set_name COLLATE collation;
  ```
+ Improperly setting parameters in a DB parameter group can have unintended adverse effects, including degraded performance and system instability\. Always exercise caution when modifying database parameters and back up your data before modifying a DB parameter group\. Try out parameter group setting changes on a test DB instance before applying those parameter group changes to a production DB instance\.
+ To determine the supported parameters for your DB engine, you can view the parameters in the DB parameter group used by the DB instance\. For more information, see [Viewing parameter values for a DB parameter group](#USER_WorkingWithParamGroups.Viewing)\.

**Topics**
+ [Creating a DB parameter group](#USER_WorkingWithParamGroups.Creating)
+ [Associating a DB parameter group with a DB instance](#USER_WorkingWithParamGroups.Associating)
+ [Modifying parameters in a DB parameter group](#USER_WorkingWithParamGroups.Modifying)
+ [Resetting parameters in a DB parameter group to their default values](#USER_WorkingWithParamGroups.Resetting)
+ [Copying a DB parameter group](#USER_WorkingWithParamGroups.Copying)
+ [Listing DB parameter groups](#USER_WorkingWithParamGroups.Listing)
+ [Viewing parameter values for a DB parameter group](#USER_WorkingWithParamGroups.Viewing)
+ [Comparing DB parameter groups](#USER_WorkingWithParamGroups.Comparing)
+ [Specifying DB parameters](#USER_ParamValuesRef)

## Creating a DB parameter group<a name="USER_WorkingWithParamGroups.Creating"></a>

You can create a new DB parameter group using the AWS Management Console, the AWS CLI, or the RDS API\.

### Console<a name="USER_WorkingWithParamGroups.Creating.CON"></a>

**To create a DB parameter group**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. Choose **Create parameter group**\.

   The **Create parameter group** window appears\.

1. In the **Parameter group family** list, select a DB parameter group family\.

1. In the **Type** list, select **DB Parameter Group**\.

1. In the **Group name** box, enter the name of the new DB parameter group\.

1. In the **Description** box, enter a description for the new DB parameter group\. 

1. Choose **Create**\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Creating.CLI"></a>

To create a DB parameter group, use the AWS CLI [https://docs.aws.amazon.com/cli/latest/reference/rds/create-db-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/rds/create-db-parameter-group.html) command\. The following example creates a DB parameter group named *mydbparametergroup* for MySQL version 5\.6 with a description of "*My new parameter group*\."

Include the following required parameters:
+ `--db-parameter-group-name`
+ `--db-parameter-group-family`
+ `--description`

To list all of the available parameter group families, use the following command:

```
aws rds describe-db-engine-versions --query "DBEngineVersions[].DBParameterGroupFamily"				
```

**Note**  
The output contains duplicates\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds create-db-parameter-group \
    --db-parameter-group-name mydbparametergroup \
    --db-parameter-group-family MySQL5.6 \
    --description "My new parameter group"
```
For Windows:  

```
aws rds create-db-parameter-group ^
    --db-parameter-group-name mydbparametergroup ^
    --db-parameter-group-family MySQL5.6 ^
    --description "My new parameter group"
```
This command produces output similar to the following:  

```
DBPARAMETERGROUP  mydbparametergroup  mysql5.6  My new parameter group
```

### RDS API<a name="USER_WorkingWithParamGroups.Creating.API"></a>

To create a DB parameter group, use the RDS API [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CreateDBParameterGroup.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CreateDBParameterGroup.html) operation\.

Include the following required parameters:
+ `DBParameterGroupName`
+ `DBParameterGroupFamily`
+ `Description`

## Associating a DB parameter group with a DB instance<a name="USER_WorkingWithParamGroups.Associating"></a>

You can create your own DB parameter groups with customized settings\. You can associate a DB parameter group with a DB instance using the AWS Management Console, the AWS CLI, or the RDS API\. You can do so when you create or modify a DB instance\.

For information about creating a DB parameter group, see [Creating a DB parameter group](#USER_WorkingWithParamGroups.Creating)\. For information about creating a DB instance, see [Creating an Amazon RDS DB instance](USER_CreateDBInstance.md)\.  For information about modifying a DB instance, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\.

**Note**  
When you change the DB parameter group associated with a DB instance, you must manually reboot the instance before the DB instance can use the new DB parameter group\.

### Console<a name="USER_WorkingWithParamGroups.Associating.CON"></a>

**To associate a DB parameter group with a DB instance**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Databases**, and then choose the DB instance that you want to modify\. 

1. Choose **Modify**\. The **Modify DB Instance** page appears\.

1. Change the **DB parameter group** setting\. 

1. Choose **Continue** and check the summary of modifications\. 

1. \(Optional\) Choose **Apply immediately** to apply the changes immediately\. Choosing this option can cause an outage in some cases\. For more information, see [Using the Apply Immediately setting](Overview.DBInstance.Modifying.md#USER_ModifyInstance.ApplyImmediately)\.

1. On the confirmation page, review your changes\. If they are correct, choose **Modify DB instance** to save your changes\. 

   Or choose **Back** to edit your changes or **Cancel** to cancel your changes\. 

### AWS CLI<a name="USER_WorkingWithParamGroups.Associating.CLI"></a>

To associate a DB parameter group with a DB instance, use the AWS CLI [ `modify-db-instance`](https://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-instance.html) command with the following options:
+ `--db-instance-identifier`
+ `--db-parameter-group-name`

The following example associates the `mydbpg` DB parameter group with the `database-1` DB instance\. The changes are applied immediately by using `--apply-immediately`\. Use `--no-apply-immediately` to apply the changes during the next maintenance window\. For more information, see [Using the Apply Immediately setting](Overview.DBInstance.Modifying.md#USER_ModifyInstance.ApplyImmediately)\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds modify-db-instance \
    --db-instance-identifier database-1 \
    --db-parameter-group-name mydbpg \
    --apply-immediately
```
For Windows:  

```
aws rds modify-db-instance ^
    --db-instance-identifier database-1 ^
    --db-parameter-group-name mydbpg ^
    --apply-immediately
```

### RDS API<a name="USER_WorkingWithParamGroups.Associating.API"></a>

To associate a DB parameter group with a DB instance, use the RDS API [ `ModifyDBInstance`](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBInstance.html) operation with the following parameters:
+ `DBInstanceName`
+ `DBParameterGroupName`

## Modifying parameters in a DB parameter group<a name="USER_WorkingWithParamGroups.Modifying"></a>

You can modify parameter values in a customer\-created DB parameter group; you can't change the parameter values in a default DB parameter group\. Changes to parameters in a customer\-created DB parameter group are applied to all DB instances that are associated with the DB parameter group\. 

Changes to some parameters are applied to the DB instance immediately without a reboot\. Changes to other parameters are applied only after the DB instance is rebooted\. The RDS console shows the status of the DB parameter group associated with a DB instance on the **Configuration** tab\. For example, if the DB instance isn't using the latest changes to its associated DB parameter group, the RDS console shows the DB parameter group with a status of **pending\-reboot**\. To apply the latest parameter changes to that DB instance, manually reboot the DB instance\.

![\[Parameter change pending reboot scenario\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/images/param-reboot.png)

### Console<a name="USER_WorkingWithParamGroups.Modifying.CON"></a>

**To modify a DB parameter group**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. In the list, choose the parameter group that you want to modify\.

1. For **Parameter group actions**, choose **Edit**\.

1. Change the values of the parameters that you want to modify\. You can scroll through the parameters using the arrow keys at the top right of the dialog box\. 

   You can't change values in a default parameter group\.

1. Choose **Save changes**\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Modifying.CLI"></a>

To modify a DB parameter group, use the AWS CLI [https://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-parameter-group.html) command with the following required options:
+ `--db-parameter-group-name`
+ `--parameters`

The following example modifies the` max_connections` and `max_allowed_packet` values in the DB parameter group named *mydbparametergroup*\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds modify-db-parameter-group \
    --db-parameter-group-name mydbparametergroup \
    --parameters "ParameterName=max_connections,ParameterValue=250,ApplyMethod=immediate" \
                 "ParameterName=max_allowed_packet,ParameterValue=1024,ApplyMethod=immediate"
```
For Windows:  

```
aws rds modify-db-parameter-group ^
    --db-parameter-group-name mydbparametergroup ^
    --parameters "ParameterName=max_connections,ParameterValue=250,ApplyMethod=immediate" ^
                 "ParameterName=max_allowed_packet,ParameterValue=1024,ApplyMethod=immediate"
```
The command produces output like the following:  

```
DBPARAMETERGROUP  mydbparametergroup
```

### RDS API<a name="USER_WorkingWithParamGroups.Modifying.API"></a>

To modify a DB parameter group, use the RDS API [ `ModifyDBParameterGroup`](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBParameterGroup.html) operation with the following required parameters:
+ `DBParameterGroupName`
+ `Parameters`

## Resetting parameters in a DB parameter group to their default values<a name="USER_WorkingWithParamGroups.Resetting"></a>

You can reset parameter values in a customer\-created DB parameter group to their default values\. Changes to parameters in a customer\-created DB parameter group are applied to all DB instances that are associated with the DB parameter group\.

When you use the console, you can reset specific parameters to their default values, but you can't easily reset all of the parameters in the DB parameter group at once\. When you use the AWS CLI or RDS API, you can reset specific parameters to their default values, and you can reset all of the parameters in the DB parameter group at once\.

Changes to some parameters are applied to the DB instance immediately without a reboot\. Changes to other parameters are applied only after the DB instance is rebooted\. The RDS console shows the status of the DB parameter group associated with a DB instance on the **Configuration** tab\. For example, if the DB instance isn't using the latest changes to its associated DB parameter group, the RDS console shows the DB parameter group with a status of **pending\-reboot**\. To apply the latest parameter changes to that DB instance, manually reboot the DB instance\.

![\[Parameter change pending reboot scenario\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/images/param-reboot.png)

**Note**  
In a default DB parameter group, parameters are always set to their default values\.

### Console<a name="USER_WorkingWithParamGroups.Resetting.CON"></a>

**To reset parameters in a DB parameter group to their default values**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. In the list, choose the parameter group\.

1. For **Parameter group actions**, choose **Edit**\.

1. Choose the parameters that you want to reset to their default values\. You can scroll through the parameters using the arrow keys at the top right of the dialog box\. 

   You can't reset values in a default parameter group\.

1. Choose **Reset** and then confirm by choosing **Reset parameters**\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Resetting.CLI"></a>

To reset some or all of the parameters in a DB parameter group, use the AWS CLI [https://docs.aws.amazon.com/cli/latest/reference/rds/reset-db-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/rds/reset-db-parameter-group.html) command with the following required option: `--db-parameter-group-name`\.

To reset all of the parameters in the DB parameter group, specify the `--reset-all-parameters` option\. To reset specific parameters, specify the `--parameters` option\.

The following example resets all of the parameters in the DB parameter group named *mydbparametergroup* to their default values\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds reset-db-parameter-group \
    --db-parameter-group-name mydbparametergroup \
    --reset-all-parameters
```
For Windows:  

```
aws rds reset-db-parameter-group ^
    --db-parameter-group-name mydbparametergroup ^
    --reset-all-parameters
```

The following example resets the `max_connections` and `max_allowed_packet` options to their default values in the DB parameter group named *mydbparametergroup*\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds reset-db-parameter-group \
    --db-parameter-group-name mydbparametergroup \
    --parameters "ParameterName=max_connections,ApplyMethod=immediate" \
                 "ParameterName=max_allowed_packet,ApplyMethod=immediate"
```
For Windows:  

```
aws rds reset-db-parameter-group ^
    --db-parameter-group-name mydbparametergroup ^
    --parameters "ParameterName=max_connections,ApplyMethod=immediate" ^
                 "ParameterName=max_allowed_packet,ApplyMethod=immediate"
```
The command produces output like the following:  

```
DBParameterGroupName  mydbparametergroup
```

### RDS API<a name="USER_WorkingWithParamGroups.Resetting.API"></a>

To reset parameters in a DB parameter group to their default values, use the RDS API [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ResetDBParameterGroup.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ResetDBParameterGroup.html) command with the following required parameter: `DBParameterGroupName`\.

To reset all of the parameters in the DB parameter group, set the `ResetAllParameters` parameter to `true`\. To reset specific parameters, specify the `Parameters` parameter\.

## Copying a DB parameter group<a name="USER_WorkingWithParamGroups.Copying"></a>

You can copy custom DB parameter groups that you create\. Copying a parameter group is a convenient solution when you have already created a DB parameter group and you want to include most of the custom parameters and values from that group in a new DB parameter group\. You can copy a DB parameter group by using the AWS Management Console, the AWS CLI [copy\-db\-parameter\-group](https://docs.aws.amazon.com/cli/latest/reference/rds/copy-db-parameter-group.html) command, or the RDS API [CopyDBParameterGroup](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CopyDBParameterGroup.html) operation\.

After you copy a DB parameter group, wait at least 5 minutes before creating your first DB instance that uses that DB parameter group as the default parameter group\. Doing this allows Amazon RDS to fully complete the copy action before the parameter group is used\. This is especially important for parameters that are critical when creating the default database for a DB instance\. An example is the character set for the default database defined by the `character_set_database` parameter\. Use the **Parameter Groups** option of the [Amazon RDS console](https://console.aws.amazon.com/rds/) or the [describe\-db\-parameters](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-parameters.html) command to verify that your DB parameter group is created\.

**Note**  
You can't copy a default parameter group\. However, you can create a new parameter group that is based on a default parameter group\.  
Currently, you can't copy a parameter group to a different AWS Region\.

### Console<a name="USER_WorkingWithParamGroups.Copying.CON"></a>

**To copy a DB parameter group**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. In the list, choose the custom parameter group that you want to copy\.

1. For **Parameter group actions**, choose **Copy**\.

1. In **New DB parameter group identifier**, enter a name for the new parameter group\.

1. In **Description**, enter a description for the new parameter group\.

1. Choose **Copy**\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Copying.CLI"></a>

To copy a DB parameter group, use the AWS CLI [ `copy-db-parameter-group`](https://docs.aws.amazon.com/cli/latest/reference/rds/copy-db-parameter-group.html) command with the following required options:
+ `--source-db-parameter-group-identifier`
+ `--target-db-parameter-group-identifier`
+ `--target-db-parameter-group-description`

The following example creates a new DB parameter group named `mygroup2` that is a copy of the DB parameter group `mygroup1`\.

**Example**  
For Linux, macOS, or Unix:  

```
aws rds copy-db-parameter-group \
    --source-db-parameter-group-identifier mygroup1 \
    --target-db-parameter-group-identifier mygroup2 \
    --target-db-parameter-group-description "DB parameter group 2"
```
For Windows:  

```
aws rds copy-db-parameter-group ^
    --source-db-parameter-group-identifier mygroup1 ^
    --target-db-parameter-group-identifier mygroup2 ^
    --target-db-parameter-group-description "DB parameter group 2"
```

### RDS API<a name="USER_WorkingWithParamGroups.Copying.API"></a>

To copy a DB parameter group, use the RDS API [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CopyDBParameterGroup.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_CopyDBParameterGroup.html) operation with the following required parameters:
+ `SourceDBParameterGroupIdentifier`
+ `TargetDBParameterGroupIdentifier`
+ `TargetDBParameterGroupDescription`

## Listing DB parameter groups<a name="USER_WorkingWithParamGroups.Listing"></a>

You can list the DB parameter groups you've created for your AWS account\.

**Note**  
Default parameter groups are automatically created from a default parameter template when you create a DB instance for a particular DB engine and version\. These default parameter groups contain preferred parameter settings and can't be modified\. When you create a custom parameter group, you can modify parameter settings\. 

### Console<a name="USER_WorkingWithParamGroups.Listing.CON"></a>

**To list all DB parameter groups for an AWS account**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

   The DB parameter groups appear in a list\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Listing.CLI"></a>

To list all DB parameter groups for an AWS account, use the AWS CLI [https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-parameter-groups.html](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-parameter-groups.html) command\.

**Example**  
The following example lists all available DB parameter groups for an AWS account\.  

```
aws rds describe-db-parameter-groups
```
The command returns a response like the following:  

```
DBPARAMETERGROUP  default.mysql5.5     mysql5.5  Default parameter group for MySQL5.5
DBPARAMETERGROUP  default.mysql5.6     mysql5.6  Default parameter group for MySQL5.6
DBPARAMETERGROUP  mydbparametergroup   mysql5.6  My new parameter group
```
The following example describes the *mydbparamgroup1* parameter group\.  
For Linux, macOS, or Unix:  

```
aws rds describe-db-parameter-groups \
    --db-parameter-group-name mydbparamgroup1
```
For Windows:  

```
aws rds describe-db-parameter-groups ^
    --db-parameter-group-name mydbparamgroup1
```
The command returns a response like the following:  

```
DBPARAMETERGROUP  mydbparametergroup1  mysql5.5  My new parameter group
```

### RDS API<a name="USER_WorkingWithParamGroups.Listing.API"></a>

To list all DB parameter groups for an AWS account, use the RDS API [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBParameterGroups.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBParameterGroups.html) operation\.

## Viewing parameter values for a DB parameter group<a name="USER_WorkingWithParamGroups.Viewing"></a>

You can get a list of all parameters in a DB parameter group and their values\.

### Console<a name="USER_WorkingWithParamGroups.Viewing.CON"></a>

**To view the parameter values for a DB parameter group**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

   The DB parameter groups appear in a list\.

1. Choose the name of the parameter group to see its list of parameters\.

### AWS CLI<a name="USER_WorkingWithParamGroups.Viewing.CLI"></a>

To view the parameter values for a DB parameter group, use the AWS CLI [https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-parameters.html](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-parameters.html) command with the following required parameter\.
+ `--db-parameter-group-name`

**Example**  
The following example lists the parameters and parameter values for a DB parameter group named *mydbparametergroup\.*  

```
aws rds describe-db-parameters --db-parameter-group-name mydbparametergroup
```
The command returns a response like the following:  

```
DBPARAMETER  Parameter Name            Parameter Value  Source           Data Type  Apply Type  Is Modifiable
DBPARAMETER  allow-suspicious-udfs                      engine-default   boolean    static      false
DBPARAMETER  auto_increment_increment                   engine-default   integer    dynamic     true
DBPARAMETER  auto_increment_offset                      engine-default   integer    dynamic     true
DBPARAMETER  binlog_cache_size         32768            system           integer    dynamic     true
DBPARAMETER  socket                    /tmp/mysql.sock  system           string     static      false
```

### RDS API<a name="USER_WorkingWithParamGroups.Viewing.API"></a>

To view the parameter values for a DB parameter group, use the RDS API [https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBParameters.html](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBParameters.html) command with the following required parameter\.
+ `DBParameterGroupName`

## Comparing DB parameter groups<a name="USER_WorkingWithParamGroups.Comparing"></a>

You can use the AWS Management Console to view the differences between two parameter groups for the same DB engine and version\.

**To compare two parameter groups**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Parameter groups**\.

1. In the list, choose the two parameter groups that you want to compare\.

1. For **Parameter group actions**, choose **Compare**\.
**Note**  
If the items you selected aren't equivalent, you can't choose **Compare**\. For example, you can't compare a MySQL 5\.6 and a MySQL 5\.7 parameter group\. You can't compare a DB parameter group and an Aurora DB cluster parameter group\.

## Specifying DB parameters<a name="USER_ParamValuesRef"></a>

DB parameter types include the following:
+ integer
+ Boolean
+ string
+ long
+ double
+ timestamp
+ object of other defined data types
+ array of values of type integer, Boolean, string, long, double, timestamp, or object

You can also specify integer and Boolean DB parameters using expressions, formulas, and functions\. 

For the Oracle engine, you can use the `DBInstanceClassHugePagesDefault` formula variable to specify a Boolean DB parameter\. See [DB parameter formula variables](#USER_FormulaVariables)\. 

For the PostgreSQL engine, you can use an expression to specify a Boolean DB parameter\. See [Boolean DB parameter expressions](#USER_ParamBooleanExpressions)\.

**Contents**
+ [DB parameter formulas](#USER_ParamFormulas)
  + [DB parameter formula variables](#USER_FormulaVariables)
  + [DB parameter formula operators](#USER_FormulaOperators)
+ [DB parameter functions](#USER_ParamFunctions)
+ [Boolean DB parameter expressions](#USER_ParamBooleanExpressions)
+ [DB parameter log expressions](#USER_ParamLogExpressions)
+ [DB parameter value examples](#USER_ParamValueExamples)

### DB parameter formulas<a name="USER_ParamFormulas"></a>

A DB parameter formula is an expression that resolves to an integer value or a Boolean value\. You enclose the expression in braces: \{\}\. You can use a formula for either a DB parameter value or as an argument to a DB parameter function\.

**Syntax**  

```
{FormulaVariable}
{FormulaVariable*Integer}
{FormulaVariable*Integer/Integer}
{FormulaVariable/Integer}
```

#### DB parameter formula variables<a name="USER_FormulaVariables"></a>

Each formula variable returns an integer or a Boolean value\. The names of the variables are case\-sensitive\.

*AllocatedStorage*  
Returns an integer representing the size, in bytes, of the data volume\.

*DBInstanceClassHugePagesDefault*  
Returns a Boolean value\. Currently, it's only supported for Oracle engines\.  
For more information, see [Enabling HugePages for an Oracle DB instance](Appendix.Oracle.CommonDBATasks.Misc.md#Oracle.Concepts.HugePages)\.

*DBInstanceClassMemory*  
Returns an integer of the number of bytes of memory allocated to the DB instance class associated with the current DB instance, less the memory used by RDS processes that manage the instance\.

*DBInstanceVCPU*  
Returns an integer representing the number of virtual central processing units \(vCPUs\) used by Amazon RDS to manage the instance\. Currently, it's only supported for the PostgreSQL engine\.

*EndPointPort*  
Returns an integer representing the port used when connecting to the DB instance\.

#### DB parameter formula operators<a name="USER_FormulaOperators"></a>

DB parameter formulas support two operators: division and multiplication\.

*Division operator: /*  
Divides the dividend by the divisor, returning an integer quotient\. Decimals in the quotient are truncated, not rounded\.  
Syntax  

```
dividend / divisor
```
The dividend and divisor arguments must be integer expressions\.

*Multiplication operator: \**  
Multiplies the expressions, returning the product of the expressions\. Decimals in the expressions are truncated, not rounded\.  
Syntax  

```
expression * expression
```
Both expressions must be integers\.

### DB parameter functions<a name="USER_ParamFunctions"></a>

You specify the arguments of DB parameter functions as either integers or formulas\. Each function must have at least one argument\. Specify multiple arguments as a comma\-separated list\. The list can't have any empty members, such as *argument1*,,*argument3*\. Function names are case\-insensitive\.

*IF*  
Returns an argument\.  
Currently, it's only supported for Oracle engines, and the only supported first argument is `{DBInstanceClassHugePagesDefault}`\. For more information, see [Enabling HugePages for an Oracle DB instance](Appendix.Oracle.CommonDBATasks.Misc.md#Oracle.Concepts.HugePages)\.  
Syntax  

```
IF(argument1, argument2, argument3)
```
Returns the second argument if the first argument evaluates to true\. Returns the third argument otherwise\.

*GREATEST*  
Returns the largest value from a list of integers or parameter formulas\.  
Syntax  

```
GREATEST(argument1, argument2,...argumentn)
```
Returns an integer\.

*LEAST*  
Returns the smallest value from a list of integers or parameter formulas\.  
Syntax  

```
LEAST(argument1, argument2,...argumentn)
```
Returns an integer\.

*SUM*  
Adds the values of the specified integers or parameter formulas\.  
Syntax  

```
SUM(argument1, argument2,...argumentn)
```
Returns an integer\.

### Boolean DB parameter expressions<a name="USER_ParamBooleanExpressions"></a>

A Boolean DB parameter expression resolves to a Boolean value of 1 or 0\. The expression is enclosed in quotation marks\.

**Note**  
Boolean DB parameter expressions are only supported for the PostgreSQL engine\.

**Syntax**  

```
"expression operator expression"
```
Both expressions must resolve to integers\. An expression can be the following:  
+ integer constant
+ DB parameter formula
+ DB parameter function
+ DB parameter variable

Boolean DB parameter expressions support the following inequality operators:

*The greater than operator: >*  
Syntax  

```
"expression > expression"
```

*The less than operator: <*  
Syntax  

```
"expression < expression"
```

* The greater than or equal to operators: >=, =>*  
Syntax  

```
"expression >= expression"
"expression => expression"
```

*The less than or equal to operators: <=, =<*  
Syntax  

```
"expression <= expression"
"expression =< expression"
```

**Example using a Boolean DB parameter expression**  
The following Boolean DB parameter expression example compares the result of a parameter formula with an integer to modify the Boolean DB parameter `wal_compression` for a PostgreSQL DB instance\. The parameter expression compares the number of vCPUs with the value 2\. If the number of vCPUs is greater than 2, then the `wal_compression` DB parameter is set to true\.  

```
aws rds modify-db-parameter-group --db-parameter-group-name group-name \
--parameters "ParameterName=wal_compression,ParameterValue=\"{DBInstanceVCPU} > 2\" "
```

### DB parameter log expressions<a name="USER_ParamLogExpressions"></a>

You can set an integer DB parameter value to a log expression\. You enclose the expression in braces: \{\}\. For example:

```
{log(DBInstanceClassMemory/8187281418)*1000}
```

The `log` function represents log base 2\. This example also uses the `DBInstanceClassMemory` formula variable\. See [DB parameter formula variables](#USER_FormulaVariables)\. 

**Note**  
Currently, you can't specify the MySQL `innodb_log_file_size` parameter with any value other than an integer\.

### DB parameter value examples<a name="USER_ParamValueExamples"></a>

These examples show using formulas, functions, and expressions for the values of DB parameters\.

**Note**  
DB Parameter functions are currently supported only in the console and aren't supported in the AWS CLI\.

**Warning**  
Improperly setting parameters in a DB parameter group can have unintended adverse effects\. These might include degraded performance and system instability\. Use caution when modifying database parameters and back up your data before modifying your DB parameter group\. Try out parameter group changes on a test DB instance, created using point\-in\-time\-restores, before applying those parameter group changes to your production DB instances\. 

**Example using the DB parameter function GREATEST**  
You can specify the `GREATEST` function in an Oracle processes parameter\. Use it to set the number of user processes to the larger of either 80 or `DBInstanceClassMemory` divided by 9,868,951\.  

```
GREATEST({DBInstanceClassMemory/9868951},80)
```

**Example using the DB parameter function LEAST**  
You can specify the `LEAST` function in a MySQL `max_binlog_cache_size` parameter value\. Use it to set the maximum cache size a transaction can use in a MySQL instance to the lesser of 1 MB or `DBInstanceClass`/256\.  

```
LEAST({DBInstanceClassMemory/256},10485760)
```