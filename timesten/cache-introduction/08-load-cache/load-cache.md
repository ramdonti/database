# Load Oracle data into the cache

## Introduction

In this lab, you will load data from the Oracle tables into the TimesTen cache tables. This action will also activate the AUTOREFRESH mechanism which will periodically refresh the cache with any changes that have occurred in the Oracle database.

**Estimated Lab Time:** 6 minutes

### Objectives

- Load the APPUSER and OE cache groups

This task is accomplished using SQL statements, so can be easily performed from application code if required.

### Prerequisites

This lab assumes that you:

- Have completed all the previous labs in this workshop, in sequence.
- Have an open terminal session in the workshop compute instance, either via NoVNC or SSH, and that session is logged into the TimesTen host (tthost1).

## Task 1: Load the APPUSER cache group

As you saw in the previous lab, when a READONLY cache group is first created its tables are empty and the autorefresh mechanism is in a paused state.

Loading the cache group populates the cache tables with the data from the Oracle database and also activates the autorefresh mechanism. The load occurs in such a manner that if any changes occur in Oracle while the load is in progress, those changes will be captured and then autorefreshed to TimesTen once the load is completed.

Load the APPUSER.CG\_VPN\_USERS cache group (1 million rows) and then examine the cach group and table.

1. Connect to the cache as the user **appuser**:

```
<copy>
ttIsql "DSN=sampledb;UID=appuser;PWD=appuser;OraclePWD=appuser"
</copy>
```

```
Copyright (c) 1996, 2022, Oracle and/or its affiliates. All rights reserved.
Type ? or "help" for help, type "exit" to quit ttIsql.

connect "DSN=sampledb;UID=appuser;PWD=********;OraclePWD=********";
Connection successful: DSN=sampledb;UID=appuser;DataStore=/tt/db/sampledb;DatabaseCharacterSet=AL32UTF8;ConnectionCharacterSet=AL32UTF8;LogFileSize=256;LogBufMB=256;PermSize=1024;TempSize=256;OracleNetServiceName=ORCLPDB1;
(Default setting AutoCommit=1)
Command>
```

2. Load the cache group:

```
<copy>
LOAD CACHE GROUP appuser.cg_vpn_users COMMIT EVERY 1024 ROWS;
</copy>
```

```
1000000 cache instances affected.
```

3. Display the cache group details:

```
<copy>
cachegroups cg_vpn_users;
</copy>
```

```
Cache Group APPUSER.CG_VPN_USERS:

  Cache Group Type: Read Only
  Autorefresh: Yes
  Autorefresh Mode: Incremental
  Autorefresh State: On
  Autorefresh Interval: 2 Seconds
  Autorefresh Status: ok
  Aging: No aging defined

  Root Table: APPUSER.VPN_USERS
  Table Type: Read Only

1 cache group found.
```

Note that the status of autorefresh has now changed to  **On**.


4. Display the cache group tables:

```
<copy>
tables;
</copy>
```

```
  APPUSER.VPN_USERS
1 table found. 
```

5. Check the row count:

```
<copy>
select count(*) from vpn_users;
</copy>
```

```
< 1000000 >
1 row found.
```

6. Update optimizer statistics for all the tables in the APPUSER schema:

```
<copy>
statsupdate;
</copy>
```

7. Exit from ttIsql:

```
<copy>
quit
</copy>
```

```
Disconnecting...
Done.
```


## Task 2: Load the OE cache groups

Now do the same for the OE schema cache groups.

1. Connect to the cache as the OE user:

```
<copy>
ttIsql "DSN=sampledb;UID=oe;PWD=oe;OraclePWD=oe"
</copy>
```

```
Copyright (c) 1996, 2022, Oracle and/or its affiliates. All rights reserved.
Type ? or "help" for help, type "exit" to quit ttIsql.

connect "DSN=sampledb;UID=oe;PWD=********;OraclePWD=********";
Connection successful: DSN=sampledb;UID=oe;DataStore=/tt/db/sampledb;DatabaseCharacterSet=AL32UTF8;ConnectionCharacterSet=AL32UTF8;LogFileSize=256;LogBufMB=256;PermSize=1024;TempSize=256;OracleNetServiceName=ORCLPDB1;
(Default setting AutoCommit=1)
Command> 
```

2. Load the CG_PROMOTIONS cache group:

```
<copy>
LOAD CACHE GROUP oe.cg_promotions COMMIT EVERY 1024 ROWS;
</copy>
```

```
2 cache instances affected.
```

3. Load the CG_PROD_INVENTORY cache group:

```
<copy>
LOAD CACHE GROUP oe.cg_prod_inventory COMMIT EVERY 1024 ROWS;
</copy>
```

```
288 cache instances affected.
```

4. Load the CG_CUST_ORDERS cache group:

```
<copy>
LOAD CACHE GROUP oe.cg_cust_orders COMMIT EVERY 1024 ROWS;
</copy>
```

```
319 cache instances affected.
```

5. Update optimizer statistics for all the tables in the OE schema:

```
<copy>
statsupdate;
</copy>
```

6. Display the cache group tables:

```
<copy>
tables;
</copy>
```

```
  OE.CUSTOMERS
  OE.INVENTORIES
  OE.ORDERS
  OE.ORDER_ITEMS
  OE.PRODUCT_DESCRIPTIONS
  OE.PRODUCT_INFORMATION
  OE.PROMOTIONS
7 tables found.
```

7. Check the row count for CUSTOMERS:

```
<copy>
select count(*) from customers;
</copy>
```

```
< 319 >
1 row found.
```

8. Check the row count for INVENTORIES:

```
<copy>
select count(*) from inventories;
</copy>
```

```
< 1112 >
1 row found.
```

9. Check the row count for ORDERS:

```
<copy>
select count(*) from orders;
</copy>
```

```
< 105 >
1 row found.
```

10. Check the row count for ORDER_ITEMS:

```
<copy>
select count(*) from order_items;
</copy>
```

```
< 665 >
1 row found.
```

11. Check the row count for PRODUCT_DESCRIPTIONS:

```
<copy>
select count(*) from product_descriptions;
</copy>
```

```
< 8639 >
1 row found.
```

12. Check the row count for PRODUCT_INFORMATION:

```
<copy>
select count(*) from product_information;
</copy>
```

```
< 288 >
1 row found.
```

13. Check the row count for PROMOTIONS:

```
<copy>
select count(*) from promotions;
</copy>
```

```
< 2 >
1 row found.
```

14. Quit out of ttIsql:

```
<copy>
quit
</copy>
```

```
Disconnecting...
Done.
```

You can now *proceed to the next lab*. 

Keep your terminal session to tthost1 open for use in the next lab.

## Acknowledgements

* **Author** - Chris Jenkins, Senior Director, TimesTen Product Management
* **Contributors** -  Doug Hood & Jenny Bloom, TimesTen Product Management
* **Last Updated By/Date** - Chris Jenkins, July 2022

