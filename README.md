# pgadmin4-oc
pgAdmin4 for OpenShift

### Introduction

Default image [dpage/pgadmin4](https://hub.docker.com/r/dpage/pgadmin4) doesn't work fine without root account, so sometimes, due to some security restriction, OpenShift doesn't allow run containers with root account. So pgadmin4 can't start with this error:

```
/entrypoint.sh: line 8: can't create /pgadmin4/config_distro.py: Permission denied
WARNING: Failed to set ACL on the directory containing the configuration database: [Errno 1] Operation not permitted: '/var/lib/pgadmin'
Traceback (most recent call last):
  File "run_pgadmin.py", line 4, in <module>
    from pgAdmin4 import app
  File "/pgadmin4/pgAdmin4.py", line 109, in <module>
    app = create_app()
  File "/pgadmin4/pgadmin/__init__.py", line 247, in create_app
    fh = logging.FileHandler(config.LOG_FILE, encoding='utf-8')
  File "/usr/local/lib/python3.7/logging/__init__.py", line 1087, in __init__
    StreamHandler.__init__(self, self._open())
  File "/usr/local/lib/python3.7/logging/__init__.py", line 1116, in _open
    return open(self.baseFilename, self.mode, encoding=self.encoding)
PermissionError: [Errno 13] Permission denied: '/var/log/pgadmin/pgadmin4.log'
sudo: unable to change to root gid: Operation not permitted
sudo: unable to initialize policy plugin
[2020-04-07 11:49:11 +0000] [1] [INFO] Starting gunicorn 19.9.0
[2020-04-07 11:49:11 +0000] [1] [INFO] Listening at: http://[::]:80 (1)
[2020-04-07 11:49:11 +0000] [1] [INFO] Using worker: threads
[2020-04-07 11:49:11 +0000] [21] [INFO] Booting worker with pid: 21
WARNING: Failed to set ACL on the directory containing the configuration database: [Errno 1] Operation not permitted: '/var/lib/pgadmin'
[2020-04-07 11:49:11 +0000] [21] [ERROR] Exception in worker process
Traceback (most recent call last):
  File "/usr/local/lib/python3.7/site-packages/gunicorn/arbiter.py", line 583, in spawn_worker
    worker.init_process()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/workers/gthread.py", line 104, in init_process
    super(ThreadWorker, self).init_process()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/workers/base.py", line 129, in init_process
    self.load_wsgi()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/workers/base.py", line 138, in load_wsgi
    self.wsgi = self.app.wsgi()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/app/base.py", line 67, in wsgi
    self.callable = self.load()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/app/wsgiapp.py", line 52, in load
    return self.load_wsgiapp()
  File "/usr/local/lib/python3.7/site-packages/gunicorn/app/wsgiapp.py", line 41, in load_wsgiapp
    return util.import_app(self.app_uri)
  File "/usr/local/lib/python3.7/site-packages/gunicorn/util.py", line 350, in import_app
    __import__(module)
  File "/pgadmin4/run_pgadmin.py", line 4, in <module>
    from pgAdmin4 import app
  File "/pgadmin4/pgAdmin4.py", line 109, in <module>
    app = create_app()
  File "/pgadmin4/pgadmin/__init__.py", line 247, in create_app
    fh = logging.FileHandler(config.LOG_FILE, encoding='utf-8')
  File "/usr/local/lib/python3.7/logging/__init__.py", line 1087, in __init__
    StreamHandler.__init__(self, self._open())
  File "/usr/local/lib/python3.7/logging/__init__.py", line 1116, in _open
    return open(self.baseFilename, self.mode, encoding=self.encoding)
PermissionError: [Errno 13] Permission denied: '/var/log/pgadmin/pgadmin4.log'
```
### Comments
This docker image should run as user 1000720000:
```
...
spec:
  securityContext:
    runAsUser: 1000720000
...
```

If you have another uids - should rebuild docker image

### Docker image
Docker image: [es1n/pgadmin4-oc](https://hub.docker.com/repository/docker/es1n/pgadmin4-oc)
