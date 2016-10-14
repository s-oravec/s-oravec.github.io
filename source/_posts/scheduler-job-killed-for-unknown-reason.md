title: Scheduler Job killed for unknown reason
tags:
  - DBMS_SCHEDULER
  - PGA
date: 2016-10-14 10:33:56
---


Sometimes when `DBMS_SCHEDULER` job is `STOPPED` and the `USER_SCHEDULER_JOB_LOG.ADDITIONAL_INFO` says only that `"Job slave process was terminated"` you may find the real reason in `ERR$` table 

```
ORA-00028: your session has been killedORA-04036: PGA memory used by the instance exceeds PGA_AGGREGATE_LIMIT```

if your session has been killed during execution of sme DML statement.