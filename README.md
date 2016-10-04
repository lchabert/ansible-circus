Role Name
=========

Ansible role to install and configure circus. Circus is an app manager. More informations: https://circus.readthedocs.io/en/latest/

Requirements
------------

Pip package must be installed on host.
Python requierement: 2.6, 2.7, 3.2 or 3.3
 

Role Variables
--------------

| Variable      | Type  | Default value |Description |
| ------------- | -----:|--------------:|-----------:|
| circus_startup_enabled| Boolean | False | Circus process should be started on startup ?|
| circus_httpd_enable| Boolean |False| http socket must be opened ? (for statistics)|
| circus_statsd_enable | Boolean | False | statsd must be enabled ? (for statistics)|
| circus_config_dir | String | /etc/circus/circus.d| Directory to store circus application specific config files.|
| circus_config_file | String | /etc/circus/circus.ini| Default config file to store globales and shared values beetween apps.|
| circus_apps | List | [] | List of applications and configurations.|
| circus_sockets | List | [] | List of sockets and configurations.|
| circus_remove_sockets| List | []| List of sockets to remove on host.|
| circus_remove_apps| List | []| List of apps to remove on host.|
| circus_systemd_path| String| /usr/lib/systemd/system/circus.service| Path where to store systemd startup file|

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: lchabert.circus
```

```yaml
circus_sockets:
  app1:
     - host = {{ansible_default_ipv4["address"]}}
     - port = 8001

circus_apps:
  order:
     - working_dir = /opt/app1
     - cmd = chaussette --backend waitress app1.wsgi.application --fd $(circus.sockets.app1)
     - uid = userapp1
     - gid = groupapp1
     - copy_env = True
     - virtualenv = {{virtualenv_dir}}
     - numprocesses = 3
     - use_sockets = True
     - virtualenv_py_ver = 3.4
     - stdout_stream.class = FileStream
     - stdout_stream.filename = {{log_dir}}/stdout.log
     - stdout_stream.max_bytes = 1073741
     - stdout_stream.backup_count = 2
     - stderr_stream.class = FileStream
     - stderr_stream.filename = {{log_dir}}/stderr.log
     - stderr_stream.max_bytes = 1073741
     - stderr_stream.backup_count = 2
```

License
-------

MIT
