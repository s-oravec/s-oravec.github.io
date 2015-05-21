title: Oracle OpenSource pains
date: 2015-05-14
tags:
- Oracle
- Open Source
---

Last couple months spent developing JavaScript applications made me think about tools/applications that I would need for same comfort of development in PL/SQL. It always came down to one thing - packages = module = components.

The best name would be **module** as package has a different meaning in Oracle world.

By module I mean

- bundle of
    - stored procedures
    - DDL & DML scripts
    - install/uninstall/upgrade SQL*Plus scripts

There is no common, industry standard how to create modules of Oracle DB code (or am I missing something?). Oracle is developing the Cart thing, which is currently exposed through Oracle SQL Developer and sadly the only way to use (in SQL Developer) is to deploy it to Oracle Cloud.

Also what we need is definition of the way how to package, distribute, install/uninstall modules into your code base and then into Oracle Database. How to define inter-module dependencies and all that stuff that is provided by e.g. [npmjs](http://www.npmjs.com) - which is great source of inspiration for me.

I'm working on draft for such a spec, which is based on npmjs' definition for `package.json`. You can find it [here](https://github.com/s-oravec/oradb_module_specification). Any comments/PRs are greatly welcomed.
