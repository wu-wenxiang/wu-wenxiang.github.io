---
layout:         post
title:          OpenStack 问题调试
category:       blog
description:    通过一个典型的 Cinder 问题展示 OpenStack 问题排错流程
---

## Cinder 服务访问异常

### 1.1 问题现象

![](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/409ee9e7cf974eebb762f5b55318e882-cinder-error-500.png)

Chrome 中对请求右键，Copy as cURL

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

注意，如果从本文中复制上面的 curl 命令发过去，返回会是正确的（写小结时发现的），如下：

```json
{"services": [{"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node03.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node01.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-scheduler", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "node02.test.local", "disabled_reason": null}, {"status": "enabled", "binary": "cinder-volume", "zone": "nova", "frozen": false, "state": "up", "updated_at": "2021-02-20T10:24:00.000000", "cluster": null, "host": "control@rbd-1", "replication_status": "disabled", "active_backend_id": null, "disabled_reason": null, "backend_state": "up"}, {"status": "enabled", "binary": "cinder-backup", "zone": "nova", "state": "up", "updated_at": "2021-02-20T10:24:01.000000", "cluster": null, "host": "control", "disabled_reason": null}]}
```

原因是从这里复制之后，`openstack-api-version: volume 3.59` 中 volume 和 3.59 之间的空格的 ASCII 码不再是原来的 160，而是变成了正常的 32。160 不能被 split，这是此问题的 root cause。

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

![](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/409ee9e7cf974eebb762f5b55318e882-cinder-error-mysql.png)

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

Call Stack 很明显，`service, volume_version = hdr.split()` 有问题，`ValueError: need more than 1 value to unpack` string split 后，返回了长度为 1 的字符串。

接下来尝试加 log：

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

会发现 LOG.error 直接打印 hdr 打印不出来，但是 type / len 又没错。从 ord 看，"volume 3.59" 中间的空格是 160，不是常见的 32。查资料，160 是页面上的 `&nbsp;` 所产生的空格。所以 root cause 应该是前端代码没有正确 encode 导致的。参考 [Difference between &#32; and &nbsp;](https://stackoverflow.com/questions/11984029/difference-between-32-and-nbsp)

#### 1.2.4 附带问题

发现 openstack-api-version 改成 x-openstack-api-version 也能 fix 问题，并且 debug 时被意外值干扰，以为 160 会被转换成 32，后来试了多次没有重现（应该是被其它请求干扰）

Research 源码，发现没有 x-openstack-api-version，只有 openstack-api-version。

刚开始猜想是不是 HAProxy 会对 x- 开头的 header 做 encode 处理？但后来发现现象是巧合导致的误判。加 x- 之后，就是另一个 header 了，加 y- 也一样。

Google，`"openstack-api-version" | "x-openstack-api-version"`，绝大多数都是 "openstack-api-version"，"x-openstack-api-version" 只有寥寥几个，但诡异的是，这几个里面居然有官方文档 [https://docs.openstack.org/doc-contrib-guide/api-guides.html](https://docs.openstack.org/doc-contrib-guide/api-guides.html)，但仅此一处，其它官方文档里未见 `x-`

![](https://raw.githubusercontent.com/wu-wenxiang/Media-WebLink/master/qiniu/409ee9e7cf974eebb762f5b55318e882-openstack-api-x-openstack-api-version.png)

应该是巧合，没有深究。

### 1.3 问题结论

1. 根本原因是前端发 request 时，header 里没有 encode 导致后端 split 逻辑不能正常处理
1. OpenStack 代码这里处理不善，前端错误不应返回 500，有改善空间

