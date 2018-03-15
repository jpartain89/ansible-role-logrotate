# jpartain89.Logrotate

| ** Travis-CI ** |
| -------------- |
| [![Build Status](https://travis-ci.org/jpartain89/ansible-role-logrotate.svg?branch=master)](https://travis-ci.org/jpartain89/ansible-role-logrotate) |

Installs Logrotate on Debian-Based systems and creates/configures the logrotate files

## Role Variables

``logrotate_conf_dir`` is the location you want these config files to be stored.

You'll want to build a dictionary for each logrotate config file.
  ``name`` is the name of the config file
  ``path`` is the specific log files path
  ``options`` is a list of the options within the file
  ``scripts`` is not a required part, but if you want to include ``prerotate`` or ``postrotate`` scripts, simply see the example below. The template will auto-fill the ``endscript`` line for you.

```
logrotate_files:
  - name: bind9
    path:
      - /var/log/bind/*
    options:
      - daily
      - 'rotate 7'
      - compress
      - delaycompress
      - notifempty
      - missingok
      - shred
      - sharedscripts
    scripts:
      postrotate: '/etc/init.d/bind9 reload >/dev/null'
```

## Example Playbook

- hosts: servers

  vars:
    logrotate_conf_dir: /etc/logrotate.d
    logrotate_files:
      - name: bind9
        path:
          - /var/log/bind/*
        options:
          - daily
          - 'rotate 7'
          - compress
          - delaycompress
          - notifempty
          - missingok
          - shred
          - sharedscripts
        scripts:
          postrotate: '/etc/init.d/bind9 reload >/dev/null'

  roles:
    - jpartain89.logrotate

## License

GPLv3

## Author Information

Justin Partain and JPCDI, github.com/jpartain89
