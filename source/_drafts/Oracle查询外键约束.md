---
title: Oracle查询外键约束
tags:
---

查询外键约束

```
select * from user_constraints;
```

禁用某个约束

```
alter table BBB disable constraint YYY cascade;
```

启用某个约束

```
alter table BBB enable constraint YYY;
```

