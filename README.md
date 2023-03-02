# chart-ccd

Product chart for Work Allocation.

Please note you must set the postgres password as the work allocation task management API needs to run as the admin user.

```
  auth:
    username: '{{ tpl .Values.global.postgresUsername $}}'
    password: '{{ tpl .Values.global.postgresPassword $}}'
    postgresqlPassword: '{{ tpl .Values.global.postgresPassword $}}'
```
