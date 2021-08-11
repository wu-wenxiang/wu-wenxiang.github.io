---
layout:         post
title:          OpenStack 问题调试
category:       blog
description:    通过一个典型的 Cinder 问题展示 OpenStack 问题排错流程
---

## Cinder 服务访问异常

### 1.1 问题现象

Web 页面上访问 cinder 服务报错 500

![](/images/weblink/409ee9e7cf974eebb762f5b55318e882-cinder-error-500.png)

Chrome 开发者工具，选择对应的 API 请求，右键，Copy as cURL

```bash
curl 'https://172.20.154.250/api/openstack/regionone/cinder/v3/4f6597fc276246b78b854d5044b020ed/os-services?all_projects=true' \
  -H 'authority: 172.20.154.250' \
  -H 'sec-ch-ua: "Chromium";v="88", "Google Chrome";v="88", ";Not A Brand";v="99"' \
  -H 'openstack-api-version: volume 3.59' \
  -H 'x-auth-token: gAAAAABgMK_8jSs6dKtCyzSr55zQmUTYqGUt3C9vzXUNnYttJLrZ-331cSpuz6tV54T1Cg4XJHPU70opaRcVK1B6LmTgq_IT8J37dV5Uu9UjoPFqWhJwO1U1TfrB0NNPlJH9hrHjyI2zU1bCrq6k079sgHwRBVTrs9nacFgBR0ykBMHMyoRaUgg2c-BZnYnr7aKHRb63mMCL' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36' \
  -H 'content-type: application/json' \
  -H 'accept: */*' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-dest: empty' \
  -H 'referer: https://172.20.154.250/configuration-admin/info?tab=cinderService' \
  -H 'accept-language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7' \
  -H 'cookie: session=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJrZXlzdG9uZV90b2tlbiI6ImdBQUFBQUJnTUtfOGpTczZkS3RDeXpTcjU1elFtVVRZcUdVdDNDOXZ6WFVObll0dEpMclotMzMxY1NwdXo2dFY1NFQxQ2c0WEpIUFU3MG9wYVJjVksxQjZMbVRncV9JVDhKMzdkVjVVdTlVam9QRnFXaEp3TzFVMVRmckIwTk5QbEpIOWhySGp5STJ6VTFiQ3JxNmswNzlzZ0h3UkJWVHJzOW5hY0ZnQlIweWtCTUhNeW9SYVVnZzJjLUJabllucjdhS0hSYjYzbU1DTCIsInJlZ2lvbiI6IlJlZ2lvbk9uZSIsImV4cCI6MTYxMzgwNzExNiwidXVpZCI6IjAyNzczNDQ3ZjhlMzQwMzJiZmNiMTRkOGE2YWFiNGU2In0.SOREmes6dIA2lCOIWq1sABkfHJ6pyH2m1kEIxzF15ls' \
  --compressed \
  --insecure
```

返回是：

```json
{"computeFault": {"message": "The server has either erred or is incapable of performing the requested operation.", "code": 500}}
```

注意，如果从本文中复制上面的 curl 命令发过去，返回会是正确的（写小结时才发现的），如下：

```json
{"services": [{"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node03.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node01.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node02.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-volume", "zone": "nova", "frozen": false, "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "control@rbd-1", "replication_status": "disabled", "active_backend_id": null, "disabled_reason": null, "backend_state": "up"}, {"status": "enabled", "binary": "cinder-backup", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:01.000000", "cluster": null, "host": "control", "disabled_reason": null}]}
```

原因是从这里复制之后，`openstack-api-version: volume 3.59` 中 volume 和 3.59 之间的空格的 ASCII 码不再是原来的 194 160，而是变成了正常的 32。194 160 不能被 split，这是此问题的 root cause。

### 1.2 问题分析

#### 1.2.1 确认服务和日志

看 Cinder 日志，Cinder 4 个服务，API / Manager / Scheduler / Volume

```console
[root@node02 ~]# docker ps | grep cinder
5e095c46fbdb   172.20.154.10:4000/kolla/centos-source-cinder-backup:iaas-8.1.0               "dumb-init --single-…"   17 hours ago   Up 2 hours                   cinder_backup
87b0d0497722   172.20.154.10:4000/kolla/centos-source-cinder-volume:iaas-8.1.0               "dumb-init --single-…"   17 hours ago   Up 2 hours                   cinder_volume
5dcd9a5752ce   172.20.154.10:4000/kolla/centos-source-cinder-scheduler:iaas-8.1.0            "dumb-init --single-…"   17 hours ago   Up 2 hours                   cinder_scheduler
80518efb548d   172.20.154.10:4000/kolla/centos-source-cinder-api:iaas-8.1.0                  "dumb-init --single-…"   17 hours ago   Up About an hour             cinder_api
```

对应的日志：

```console
[root@node02 ~]# ls /var/log/kolla/cinder/
cinder-api-access.log  cinder-api.log  cinder-backup.log  cinder-scheduler.log  cinder-volume.log  cinder-wsgi.log
```

这个问题应该是看 API 服务对应的日志，cinder-api-access.log、cinder-api.log 和 cinder-wsgi.log。

#### 1.2.2 重启服务，排除 MySQL Lost Connection 干扰

刚开始看的时候，发现 cinder-shceduler.log 和 cinder-volume.log 等日志里有 MySQL lost connection。

```
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines [req-4dd3231a-09d0-4e6b-a001-224a1f33aa1e - - - - -] Database connection was found disconnected; reconnecting: DBConnectionError: (pymysql.err.OperationalError) (2013, 'Lost connection to MySQL server during query')
[SQL: SELECT 1]
(Background on this error at: http://sqlalche.me/e/e3q8)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines Traceback (most recent call last):
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/oslo_db/sqlalchemy/engines.py", line 73, in _connect_ping_listener
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     connection.scalar(select([1]))
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 920, in scalar
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     return self.execute(object_, *multiparams, **params).scalar()
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 988, in execute
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     return meth(self, multiparams, params)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/sql/elements.py", line 287, in _execute_on_connection
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     return connection._execute_clauseelement(self, multiparams, params)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 1107, in _execute_clauseelement
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     distilled_params,
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 1253, in _execute_context
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     e, statement, parameters, cursor, context
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 1471, in _handle_dbapi_exception
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     util.raise_from_cause(newraise, exc_info)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/util/compat.py", line 398, in raise_from_cause
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     reraise(type(exception), exception, tb=exc_tb, cause=cause)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/base.py", line 1249, in _execute_context
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     cursor, statement, parameters, context
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib64/python2.7/site-packages/sqlalchemy/engine/default.py", line 552, in do_execute
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     cursor.execute(statement, parameters)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/cursors.py", line 170, in execute
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     result = self._query(query)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/cursors.py", line 328, in _query
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     conn.query(q)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/connections.py", line 517, in query
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     self._affected_rows = self._read_query_result(unbuffered=unbuffered)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/connections.py", line 732, in _read_query_result
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     result.read()
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/connections.py", line 1075, in read
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     first_packet = self.connection._read_packet()
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/connections.py", line 657, in _read_packet
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     packet_header = self._read_bytes(4)
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines   File "/var/lib/kolla/venv/lib/python2.7/site-packages/pymysql/connections.py", line 707, in _read_bytes
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines     CR.CR_SERVER_LOST, "Lost connection to MySQL server during query")
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines DBConnectionError: (pymysql.err.OperationalError) (2013, 'Lost connection to MySQL server during query')
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines [SQL: SELECT 1]
2021-02-20 02:26:50.375 7 ERROR oslo_db.sqlalchemy.engines (Background on this error at: http://sqlalche.me/e/e3q8)
```

登陆到对应的 cinder-scheduler 容器中，用 telnet 访问 mysql 3306 端口，发现可以通。这里要注意，默认登陆进 cinder-scheduler 容器是 cinder 用户，需要 `docker exec --user root -it cinder-scheduler /bin/bash` 切换成 root 用户登陆进去，然后 `yum update`，`yum install telnet`，然后根据 /etc/cinder/cinder.config 里的 `connection = mysql+pymysql://cinder:a0NpMDCpO9kYXXy3DSxMiIAMbFRdh4AYHCRM5fpW@172.20.154.249:3306/cinder` 得知 mysql IP 和端口，telnet 确认端口通。

```console
(cinder-scheduler)[root@node01 /]# telnet 172.20.154.249 3306
Trying 172.20.154.249...
Connected to 172.20.154.249.
Escape character is '^]'.
]
```

既然端口通，mysql 应该没问题，为排除干扰，重启一下 cinder 服务。看这个报错还有没有。

先修改配置文件，否则不会重启容器

```console
[root@iaas-8-deploy ~]# ansible -i /etc/ansible/hosts/ control -m shell -a 'mv /etc/kolla/cinder-scheduler/cinder.conf /etc/kolla/cinder-scheduler/cinder.conf.org'
[WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details
node02 | CHANGED | rc=0 >>
node01 | CHANGED | rc=0 >>
node03 | CHANGED | rc=0 >>
```

然后重新部署 cinder：`kolla-ansible -i /etc/ansible/hosts/ -t cinder deploy`

之后检查一下容器是否重新启动：

```console
[root@iaas-8-deploy ~]# ansible -i /etc/ansible/hosts/ control -m shell -a "docker ps | grep -i cinder"
[WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details
node02 | CHANGED | rc=0 >>
5e095c46fbdb   172.20.154.10:4000/kolla/centos-source-cinder-backup:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 19 seconds                cinder_backup
87b0d0497722   172.20.154.10:4000/kolla/centos-source-cinder-volume:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 30 seconds                cinder_volume
5dcd9a5752ce   172.20.154.10:4000/kolla/centos-source-cinder-scheduler:iaas-8.1.0            "dumb-init --single-…"   14 hours ago   Up 42 seconds                cinder_scheduler
80518efb548d   172.20.154.10:4000/kolla/centos-source-cinder-api:iaas-8.1.0                  "dumb-init --single-…"   14 hours ago   Up 54 seconds                cinder_api
node01 | CHANGED | rc=0 >>
66ce5d2288b1   172.20.154.10:4000/kolla/centos-source-cinder-backup:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 18 seconds                cinder_backup
b2aa5b69c62f   172.20.154.10:4000/kolla/centos-source-cinder-volume:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 31 seconds                cinder_volume
967a856b0f63   172.20.154.10:4000/kolla/centos-source-cinder-scheduler:iaas-8.1.0            "dumb-init --single-…"   14 hours ago   Up 42 seconds                cinder_scheduler
662c6973212e   172.20.154.10:4000/kolla/centos-source-cinder-api:iaas-8.1.0                  "dumb-init --single-…"   14 hours ago   Up 54 seconds                cinder_api
node03 | CHANGED | rc=0 >>
412894a8bdf4   172.20.154.10:4000/kolla/centos-source-cinder-backup:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 18 seconds                cinder_backup
cef5ac29aac9   172.20.154.10:4000/kolla/centos-source-cinder-volume:iaas-8.1.0               "dumb-init --single-…"   14 hours ago   Up 30 seconds                cinder_volume
9134f2b9fa2b   172.20.154.10:4000/kolla/centos-source-cinder-scheduler:iaas-8.1.0            "dumb-init --single-…"   14 hours ago   Up 42 seconds                cinder_scheduler
c0760120fa60   172.20.154.10:4000/kolla/centos-source-cinder-api:iaas-8.1.0                  "dumb-init --single-…"   14 hours ago   Up 54 seconds                cinder_api
```

重启之后，没有再看到这个报错，所以暂时不予理会。

#### 1.2.3 重现问题

然后用 Chrome 里直接复制的 cURL 命令重现问题，日志在 cinder-wsgi.log

```txt
2021-02-20 16:04:09.467 22 INFO cinder.api.openstack.wsgi [req-1c1806d4-f3cb-46c4-a01c-e78094c6fbd9 c4128648bc484324a436ef95208e911e 4f6597fc276246b78b854d5044b020ed - default default] GET https://172.20.154.250:8776/v3/4f6597fc276246b78b854d5044b020ed/os-services?all_projects=true
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault [req-1c1806d4-f3cb-46c4-a01c-e78094c6fbd9 c4128648bc484324a436ef95208e911e 4f6597fc276246b78b854d5044b020ed - default default] Caught error: <type 'exceptions.ValueError'> need more than 1 value to unpack: ValueError: need more than 1 value to unpack
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault Traceback (most recent call last):
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/cinder/api/middleware/fault.py", line 85, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return req.get_response(self.application)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1314, in send
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     application, catch_exc_info=False)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1278, in call_application
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     app_iter = application(self.environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 143, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return resp(environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 129, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     resp = self.call_func(req, *args, **kw)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 193, in call_func
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return self.func(req, *args, **kwargs)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/osprofiler/web.py", line 112, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return request.get_response(self.application)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1314, in send
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     application, catch_exc_info=False)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1278, in call_application
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     app_iter = application(self.environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 129, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     resp = self.call_func(req, *args, **kw)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 193, in call_func
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return self.func(req, *args, **kwargs)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/keystonemiddleware/auth_token/__init__.py", line 341, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     response = req.get_response(self._app)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1314, in send
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     application, catch_exc_info=False)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1278, in call_application
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     app_iter = application(self.environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 143, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return resp(environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 129, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     resp = self.call_func(req, *args, **kw)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 193, in call_func
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return self.func(req, *args, **kwargs)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/keystonemiddleware/audit/__init__.py", line 149, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return req.get_response(self._application)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1314, in send
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     application, catch_exc_info=False)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/request.py", line 1278, in call_application
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     app_iter = application(self.environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 143, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return resp(environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/routes/middleware.py", line 141, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     response = self.app(environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 143, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return resp(environ, start_response)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 129, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     resp = self.call_func(req, *args, **kw)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/webob/dec.py", line 193, in call_func
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     return self.func(req, *args, **kwargs)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/cinder/api/openstack/wsgi.py", line 825, in __call__
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     request.set_api_version_request(request.url)
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault   File "/var/lib/kolla/venv/lib/python2.7/site-packages/cinder/api/openstack/wsgi.py", line 306, in set_api_version_request
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault     service, volume_version = hdr.split()
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault ValueError: need more than 1 value to unpack
2021-02-20 16:04:09.469 22 ERROR cinder.api.middleware.fault 
2021-02-20 16:04:09.473 22 INFO cinder.api.middleware.fault [req-1c1806d4-f3cb-46c4-a01c-e78094c6fbd9 c4128648bc484324a436ef95208e911e 4f6597fc276246b78b854d5044b020ed - default default] https://172.20.154.250:8776/v3/4f6597fc276246b78b854d5044b020ed/os-services?all_projects=true returned with HTTP 500
```

Call Stack 很明显，`service, volume_version = hdr.split()` 有问题，`ValueError: need more than 1 value to unpack` string split 后，返回了长度为 1 的字符串列表。

接下来尝试对 /var/lib/kolla/venv/lib/python2.7/site-packages/cinder/api/openstack/wsgi.py 加 log：

```python
for hdr in hdr_string_list:
    if VOLUME_SERVICE in hdr:
        try:
            service, volume_version = hdr.split()
        except Exception as e:
            LOG.error('xxxxxxxxxx %s xxxxxxxxx' % type(hdr))
            LOG.error('xxxxxxxxxx %s xxxxxxxxx' % len(hdr))
            LOG.error('xxxxxxxxxx %s xxxxxxxxx' % hdr)
            wwx_index = 0
            for i in hdr:
                LOG.error('xxxxxxxxxx %s: %s xxxxxxxxx' % (wwx_index, i))
                LOG.error('xxxxxxxxxx %s: %s xxxxxxxxx' % (wwx_index, ord(i)))
            LOG.error('================ %s ================' % e)
            service, volume_version = "volume 3.59".split()
        break
```

然后删除 pyc 文件 /var/lib/kolla/venv/lib/python2.7/site-packages/cinder/api/openstack/wsgi.pyc，重启容器 `docker restart cinder_api`，重现问题。会发现 LOG.error 直接打印 hdr 打印不出来（**这里非常坑，当字符串中有非可显示字符时，LOG.error 就整句不打印，让人误以为修改未生效 -_-**），但是 type / len 又没错。从 ord 看，"volume 3.59" 中间的空格是 194 160，不是常见的 32。查资料，194 160 是页面上的 `&nbsp;` 所产生的空格。所以看起来是前端代码没有正确 encode 导致的。参考 [`Difference between &#32; and &nbsp;`](https://stackoverflow.com/questions/11984029/difference-between-32-and-nbsp)，参考 [Trim whitespace ASCII character “194” from string](https://stackoverflow.com/questions/42424555/trim-whitespace-ascii-character-194-from-string)：18
It's more likely to be a two-byte 194 160 sequence, which is the UTF-8 encoding of a NO-BREAK SPACE codepoint (the equivalent of the `&nbsp;` entity in HTML).

把 Chrome 里的直接复制出来的命令贴到 Sublime Text。然后用二进制工具分析，可以看到 header 里的 volume 3.59 之间的空格确实是 194 160。换成普通空格再 curl，问题不复现。

```python
>>> a = " volume 3.59"
>>> [ord(i) for i in a] # 因为本地 python3 console，ctrl-c & ctrl-v 过来会 miss 掉 194，后来才发现的
[32, 118, 111, 108, 117, 109, 101, 160, 51, 46, 53, 57]
>>> a[1:]
'volume\xa03.59'
>>> a[1:].split() # 这里是 python3 console，split 没问题
['volume', '3.59']
```

**注意看前面的空格是 32 和后面空格却是 160**

Python3 里是可以正常 split 的，Python2 不行，并且复制文本到 Python2 console 还会多一个 194 出来。去掉 194 也一样不能 split

```python
>>> a = " volume 3.59"
>>> [ord(i) for i in a]
[32, 118, 111, 108, 117, 109, 101, 194, 160, 51, 46, 53, 57]
>>> a[1:]
'volume\xc2\xa03.59'
>>> a[1:].split() # 生产环境中是 python2，split 不能处理 160 空格
['volume\xc2\xa03.59']

>>> a = a[1:7]+a[8:]
>>> [ord(i) for i in a]
[118, 111, 108, 117, 109, 101, 160, 51, 46, 53, 57]
>>> a
'volume\xa03.59'
>>> a.split()
['volume\xa03.59']
```

在源文件中修改 debug 语句，发现 194 是正常的。因为用了 python3 console，copy 过来的时候漏掉了 194，应该是 python3 console 对 UTF-8 编码的处理逻辑

```python
for hdr in hdr_string_list:
    if VOLUME_SERVICE in hdr:
        LOG.error('xxxxxxxxxx %s xxxxxxxxx' % [ord(i) for i in hdr])
        service, volume_version = hdr.split()
        break
# log: xxxxxxxxxx [118, 111, 108, 117, 109, 101, 194, 160, 51, 46, 53, 57] xxxxxxxxx
```

如果 python3 直接处理 194，也处理也不好，因此换用 python3 未必能完全避免问题。但因为 python3 默认用 utf-8 decode，所以 194 和 160 会一起处理为空格，而不是像下面这样拼接。这应该也就是 python3 console miss 掉 194 的原因（没有在生产环境中继续验证）。猜想大概率 python3 能 fix 此问题。

```python
>>> a = a[1:7]+chr(194)+a[7:]
>>> [ord(i) for i in a]
[118, 111, 108, 117, 109, 101, 194, 160, 51, 46, 53, 57]
>>> a.split() # python3 console 处理 194
['volumeÂ', '3.59']
```

```python
# python3
>>> open('a.txt')
<_io.TextIOWrapper name='a.txt' mode='r' encoding='UTF-8'>
>>> [ord(i) for i in open('a.txt').read()]
[32, 118, 111, 108, 117, 109, 101, 160, 51, 46, 53, 57]

#python2
>>> open('a.txt')
<open file 'a.txt', mode 'r' at 0x105123660>
>>> [ord(i) for i in open('a.txt').read()]
[32, 118, 111, 108, 117, 109, 101, 194, 160, 51, 46, 53, 57]
```

可以看到同一个文件，python3 是不会解析出 194 的，应该是 194 和 160 一起 utf-8 decode 了。因此大概率 python3 能 fix 这个问题。

#### 1.2.4 附带问题

发现 `openstack-api-version` 改成 `x-openstack-api-version` 也能 fix 问题，并且 debug 时被意外值干扰，以为 194 160 会被转换成 32，后来试了多次没有重现（应该是被其它请求干扰）

Research 源码，发现没有 `x-openstack-api-version`，只有 `openstack-api-version`。

刚开始猜想是不是 HAProxy 会对 `x-` 开头的 header 做 encode 处理？但后来发现现象是巧合导致的误判。加 `x-` 之后，就是另一个 header 了，加 `y-` 也一样。

Google，`"openstack-api-version" | "x-openstack-api-version"`，绝大多数都是 "openstack-api-version"，"x-openstack-api-version" 只有寥寥几个，但诡异的是，这几个里面居然有官方文档 [https://docs.openstack.org/doc-contrib-guide/api-guides.html](https://docs.openstack.org/doc-contrib-guide/api-guides.html)，但仅此一处，其它官方文档里未见 `x-`

![](/images/weblink/409ee9e7cf974eebb762f5b55318e882-openstack-api-x-openstack-api-version.png)

应该是文档有错，没有深究。

检查之前的文档版本，也是 openstack-api-version，但前端发请求时，header 里 volume 3.59 中间的空格是 32 不是 194 160，所以这个问题应该是新版本里前端代码的问题。

```python
>>> a = " volume 3.59"
>>> [ord(i) for i in a]
[32, 118, 111, 108, 117, 109, 101, 32, 51, 46, 53, 57]
```

**前后空格都是 32**

### 1.3 问题结论

1. 根本原因是前端发 request 时，header 里没有 encode，导致原来 ASCII 32 的空格变成 UTF-8 的 `&nbsp;`（Python2 默认按 ASCII 解析为 194 160，Python3 默认按 UTF-8 解析为 160），进而导致后端（Python2） split 逻辑不能正常处理
1. OpenStack 代码这里处理不善，前端错误不应返回 500，有改善空间
1. 如果 Cinder 后端换成 python3，此问题应该不能重现。
