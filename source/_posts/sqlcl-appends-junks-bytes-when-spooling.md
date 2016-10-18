title: SQLcl appends junks bytes when spooling
date: 2016-10-18 15:02:24
tags:
- SQL*Plus
- SQLcl
---

SQLcl: Release 4.2.0.16.260.1205 Production appends some junk bytes when spooling under certain conditions.
Works fine with SQLcl: Release 4.2.0.16.175.1027 RC and SQL*Plus: Release 12.1.0.2.0 Production

```bash
1. git clone https://github.com/s-oravec/sqlsn -b bug/sqlcl-spools-junk-bytes sqlsn
2. cd sqlsn
3. sql /nolog @sqlsnrc.sql
4. hexdump sqlsn_modules/sqlsn-run/lib/command/module_config.sql
```

On both macOS and Windows (in Parallels on macOS) with SQLcl: Release 4.2.0.16.175.1027 RC and SQL*Plus: Release 12.1.0.2.0 Production

```
0000000 64 65 66 69 6e 65 20 72 75 6e 5f 6d 6f 64 75 6c
0000010 65 5f 70 61 74 68 20 3d 20 22 73 71 6c 73 6e 5f
0000020 6d 6f 64 75 6c 65 73 2f 73 71 6c 73 6e 2d 72 75
0000030 6e 22 0a                                       
0000033
```

vs.

On both macOS and Windows (in Parallels on macOS) with SQLcl: Release 4.2.0.16.260.1205 Production

```
0000000 64 65 66 69 6e 65 20 72 75 6e 5f 6d 6f 64 75 6c
0000010 65 5f 70 61 74 68 20 3d 20 22 73 71 6c 73 6e 5f
0000020 6d 6f 64 75 6c 65 73 2f 73 71 6c 73 6e 2d 72 75
0000030 6e 22 0a 1b 5b 6d 0a                           
0000037
```

(macOS and Windows dumps differs only in LF vs. CRLF)

Also there seems to be some issue with thread synchronization between host I/O and processing rest of the script.
Script continues before I/O call is returned/file is written/closed. This is especially visible when you

1. spool script created with prompts
2. call created script using @

For the script to work in SQLcl: Release 4.2.0.16.175.1027 RC, you have to "make some delay" - like calling `host :` as noop (see `sqlsn_modules/sqlsn-stack/lib/command/push.sql` in repo, lines 16,17)

I haven't noticed such a behavior in SQLcl: Release 4.2.0.15.349.0706 RC.


