
# Database Internals
## [How tables and indexes are stored in disk](#1)
## [Row-based vs Columnar-based Databases](#2)
## [Primary Key (index) and Secondary Key (index)](#3)


## <a name="1"></a>How tables and indexes are stored in disk

![alt text](../images/di1.png "")

![alt text](../images/di2.png "")

![alt text](../images/di3.png "")

![alt text](../images/di4.png "")

![alt text](../images/di5.png "")

Index 10(1,0) = Employee ID 10, Row Number 1, Page 0
![alt text](../images/di6.png "")

![alt text](../images/di7.png "")
![alt text](../images/di8.png "")
![alt text](../images/di9.png "")
![alt text](../images/di10.png "")

## <a name="2"></a>Row-based vs Columnar-based Databases

![alt text](../images/di11.png "")
![alt text](../images/di12.png "")
![alt text](../images/di13.png "")
Each grey box means block
![alt text](../images/di14.png "")
![alt text](../images/di15.png "")
![alt text](../images/di16.png "")
For this query, row-oriented database is slow
![alt text](../images/di17.png "")
![alt text](../images/di18.png "")
![alt text](../images/di19.png "")
![alt text](../images/di20.png "")
![alt text](../images/di21.png "")
![alt text](../images/di22.png "")
![alt text](../images/di23.png "")

## <a name="3"></a>Primary Key (index) and Secondary Key (index)

**Heap** 
 - Table data stored in disk without any indexes.
 - Table data is organized row by row without any order.
 - Usually it has slower access space.
 - There is no order which is maintained by default.
 - Inserted rows will just go bottom of each other.

**Clustering**
 - Usually it is associated with **primary key**, but not always
 - Organizing the table around the **key**, so be default we have to maintain the order
 - By defaults inserted rows should fit that order, that means there is a additional cost associated with ordering
 - In Oracle, it is called **Index Organized Table**
 - Heap is organized around that index
 - It is very efficient for **range queries**

**Nonclustering**
 - secondary index
 - table and index are separated data structures
 - index data structure is usually B-tree
 - table data is not ordered
 - index leafs has pointer to rows of table data
 - if you want to do the search, you have to jump to the index first, the you cand find the Row IDs -> this is the disadvantage of the secondary index









