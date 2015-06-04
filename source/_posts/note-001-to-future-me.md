title: SQL*Plus set substitution variable from select
date: 2015-06-04
tags:
- Oracle
- SQL*Plus
---

Don't waste time googling "sqlplus set substitution variable from select". It's 

````
COLUMN USER NEW_VALUE g_schema
SELECT USER FROM dual;
prompt &&g_schema
````
