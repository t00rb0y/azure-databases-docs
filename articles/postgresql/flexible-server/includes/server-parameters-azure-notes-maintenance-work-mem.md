---
title: maintenance_work_mem server parameter
description: maintenance_work_mem server parameter for Azure Database for PostgreSQL - Flexible Server.
ms.service: azure-database-postgresql
ms.subservice: flexible-server
ms.topic: include
ms.date: 10/07/2024
author: nachoalonsoportillo
ms.author: ialonso
zone_pivot_groups: postgresql-server-version
---
#### Description

`maintenance_work_mem` is a configuration parameter in PostgreSQL. It governs the amount of memory allocated for maintenance operations, such as `VACUUM`, `CREATE INDEX`, and `ALTER TABLE`. Unlike `work_mem`, which affects memory allocation for query operations, `maintenance_work_mem` is reserved for tasks that maintain and optimize the database structure.

#### Key points

* **Vacuum memory cap**: If you want to speed up the cleanup of dead tuples by increasing `maintenance_work_mem`, be aware that `VACUUM` has a built-in limitation for collecting dead tuple identifiers. It can use only up to 1 GB of memory for this process.
* **Separation of memory for autovacuum**: You can use the `autovacuum_work_mem` setting to control the memory that autovacuum operations use independently. This setting acts as a subset of `maintenance_work_mem`. You can decide how much memory autovacuum uses without affecting the memory allocation for other maintenance tasks and data definition operations.

#### Azure-specific notes
The default value for the `maintenance_work_mem` server parameter is calculated when you provision the instance of Azure Database for PostgreSQL flexible server, based on the product name that you select for its compute. Any subsequent changes of product selection to the compute that supports the flexible server won't have any effect on the default value for the `maintenance_work_mem` server parameter of that instance.

Every time you change the product assigned to an instance, you should also adjust the value for the `maintenance_work_mem` parameter according to the values in the following formula.

The formula used to compute the value of `maintenance_work_mem` is `(82.5 + log(maintenance_work_mem) + 40) * 1024`.