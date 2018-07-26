# Adventureworks for Microsoft SQL Server 2016

## Summary

The following was tested on AWS AMI ami-9a5562e5, [Microsoft Windows Server 2016 with SQL Server 2016 Web](https://aws.amazon.com/marketplace/pp/B01M6ZOVHK?qid=1532469349148&sr=0-2&ref_=srh_res_product_title), with the [Adventureworks 2014 OLTP database](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 


Note that this is the non-DW version of the database (it comes in two forms, dimensional (DW/OLAP) and normalized (OLTP)) and is therefore the online transactional version of itself, in accordance with the [3rd normal form](https://en.wikipedia.org/wiki/Database_normalization). Although more challenging to extract information, this more closely resembles live enterprise DBs and better prepares the student for work in the field.

## Installation Steps

- Download the [AdventureWorks2014.bak](./AdventureWorks2014.bak) file (contained in this repo for convenience).
- If you are using AWS, spool up an appropriately sized EC2 instance of ami-9a5562e5, [Microsoft Windows Server 2016 with SQL Server 2016 Web](https://aws.amazon.com/marketplace/pp/B01M6ZOVHK?qid=1532469349148&sr=0-2&ref_=srh_res_product_title). t2.Medium (2vCPU, 4GB RAM, 50GB HDD) was used for testing and should be appropriate for moderately sized classes (20 students or so).
- Create a keypair (.pem), decrypt the Administrator password using the connect button of the AWS EC2 console.
- Using RDP, connect to your instance using [these steps](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html?icmpid=docs_ec2_console).
- Load the Adventureworks2014.bak file onto the database server (in this tutorial's case, the EC2 instance)
- Open SQL Server Management Studio
- Right click on the 'Databases' folder and click on 'Restore Database'
- Select 'Device' for source and use the ellipsis to browse to the .bak file <img src="https://raw.githubusercontent.com/ggodreau/adventureworks_mssql/master/assets/install0.png" width="650">
- Ensure the database name in the restore plan is 'AdventureWorks2014' and does not conflict with any other databases on the server
- Click OK
- Verify the restore looks as follows <img src="https://raw.githubusercontent.com/ggodreau/adventureworks_mssql/master/assets/install1.png" width="650">

## Query Testing

To ensure the data was loaded correctly, run the following query (PK checksum) on the Product table:

```sql
USE AdventureWorks2014
GO
SELECT SUM(p.ProductID)
FROM Production.Product p
```

Expected result:
```
339212
```

And run the following query as a checksum for the total number of tables loaded into the database:

```sql
USE AdventureWorks2014
GO
SELECT COUNT(*)
FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE'
```

Expected result:
```
71
```
