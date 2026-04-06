---
tags: [book]
---
- file-processing system is supported by a conventional operating system. The system stores permanent records in various files, and it needs different application programs to extract records from, and add records to, the appropriate files. 

- Keeping organizational information in a file-processing system has a number of major disadvantages: Data redundancy and inconsistency. 
- Since different programmers create the files and application programs over a long period, the various files are likely to have different structures, and the programs may be written in several programming languages. 
- Moreover, the same information may be duplicated in several places (files). 
- This redundancy leads to higher storage and access cost.
- It may lead to data inconsistency ; that is, the various copies of the same data may no longer agree.
- Difficulty in accessing data. 

conventional file-processing environments do not allow needed data to be retrieved in a convenient and efficient manner. 
More responsive data-retrieval systems are required for general use. 
- Data isolation. Because data are scattered in various files, and files may be in different formats, writing new application programs to retrieve the appropriate data is difficult.
- Integrity problems. The data values stored in the database must satisfy certain types of consistency constraints . Developers enforce these constraints in the system by adding appropriate code in the various application programs. However, when new constraints are added, it is difficult to change the programs to enforce them. The problem is compounded when constraints involve several data items from different files. 
- Atomicity problems. A computer system, like any other device, is subject to failure. In many applications, it is crucial that, if a failure occurs, the data be restored to the consistent state that existed prior to the failure. It is difficult to ensure atomicity in a conventional file-processing system. 
- Concurrent-access anomalies. For the sake of overall performance of the system and faster response, many systems allow multiple users to update the data simultaneously.
- Security problems. Not every user of the database system should be able to access all the data. 