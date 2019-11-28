title: Please, do not write names of the units after end
date: 2019-11-28
tags:
- Oracle
- PL/SQL
---

Oh, another post took only 11 months ;) Anyways, please just don't do it, and here's another good reason why.

```sql
create or replace package pkg as
end pkg;
/

declare
    l_pkg_handle           number;
    l_pkg_modify_handle    number;
    l_pkg_transform_handle number;
    l_ddl                  clob;
begin
    l_pkg_handle := dbms_metadata.open('PACKAGE');
    dbms_metadata.set_filter(l_pkg_handle, 'NAME', 'PKG');
    dbms_metadata.set_filter(l_pkg_handle, 'SCHEMA', user);
    l_pkg_modify_handle := dbms_metadata.add_transform(l_pkg_handle, 'MODIFY');
    dbms_metadata.set_remap_param(l_pkg_modify_handle, 'REMAP_NAME', 'PKG', 'PKG2');
    l_pkg_transform_handle := dbms_metadata.add_transform(l_pkg_handle, 'DDL');
    l_ddl := dbms_metadata.fetch_clob(l_pkg_handle);
    dbms_metadata.close(l_pkg_handle);
    DBMS_OUTPUT.put_line(l_ddl);
end;
/
```

The name of the package after `end` does not get changed. 

```bash
sql> create or replace package pkg as
     end pkg;
[2019-11-28 20:43:03] completed in 17 ms
sql> declare
         l_pkg_handle           number;
         l_pkg_modify_handle    number;
         l_pkg_transform_handle number;
         l_ddl                  clob;
     begin
         l_pkg_handle := dbms_metadata.open('PACKAGE');
         dbms_metadata.set_filter(l_pkg_handle, 'NAME', 'PKG');
         dbms_metadata.set_filter(l_pkg_handle, 'SCHEMA', user);
         l_pkg_modify_handle := dbms_metadata.add_transform(l_pkg_handle, 'MODIFY');
         dbms_metadata.set_remap_param(l_pkg_modify_handle, 'REMAP_NAME', 'PKG', 'PKG2');
         l_pkg_transform_handle := dbms_metadata.add_transform(l_pkg_handle, 'DDL');
         l_ddl := dbms_metadata.fetch_clob(l_pkg_handle);
         dbms_metadata.close(l_pkg_handle);
         DBMS_OUTPUT.put_line(l_ddl);
     end;
[2019-11-28 20:43:03] completed in 65 ms
[2019-11-28 20:43:03] 
[2019-11-28 20:43:03] CREATE OR REPLACE EDITIONABLE PACKAGE "SCOTT"."PKG2" as
[2019-11-28 20:43:03] end pkg;
```
