---
tags: 
- JOURNALING
- IBM
MOC: IBMi
source: "https://www.ibm.com/docs/en/i/7.1.0?topic=integrity-journaling"
---

[[_0000 Home|Home]] | [[__0008 IBM i MOC|Back to IBMi MOC]] 

# Journaling
- The DB2® for i journal support provides an audit trail and forward and backward recovery.
- Forward recovery can be used to take an older version of a table and apply the changes logged on the journal to the table. Backward recovery can be used to remove changes logged on the journal from the table.
- When an SQL schema is created, a journal and journal receiver are created in the schema. When SQL creates the journal and journal receiver, they are only created on a user auxiliary storage pool (ASP) if the ASP clause is specified on the CREATE SCHEMA statement. However, because placing journal receivers on their own ASPs can improve performance, the person managing the journal might want to create all future journal receivers on a separate ASP.
- When a table is created into the schema, it is automatically journaled to the journal that DB2 for i created in the schema (QSQJRN). A table created in a library also has journaling started if a journal named QSQJRN exists in that library. After this point, it is your responsibility to use the journal functions to manage the journal, the journal receivers, and the journaling of tables to the journal. For example, if a table is moved into a schema, no automatic change to the journaling status occurs. If a table is restored, the normal journal rules apply. That is, if the table was journaled at save time, it is journaled to the same journal at restore time. If the table was not journaled at save time, it is not journaled at restore time.
- The journal created in the SQL schema is normally the journal used for logging all changes to SQL tables. You can, however, use the system journal functions to journal SQL tables to a different journal.
- A user can stop journaling on any table using the journal functions, but doing so prevents an application from running under commitment control. If journaling is stopped on a parent table of a referential constraint with a delete rule of NO ACTION, CASCADE, SET NULL, or SET DEFAULT, all update and delete operations will be prevented. Otherwise, an application is still able to function if you have specified COMMIT(*NONE); however, this does not provide the same level of integrity that journaling and commitment control provide.