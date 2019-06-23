[![Build Status](https://travis-ci.com/nkakouros-original/ansible-role-elasticsearch.svg?branch=master)](https://travis-ci.com/nkakouros-original/ansible-role-elasticsearch)
[![Galaxy](https://img.shields.io/badge/galaxy-nkakouros.elasticsearch-blue.svg)](https://galaxy.ansible.com/nkakouros/elasticsearch/)

ansible-role-elasticsearch
=========

Installs and configures Elasticsearch.

Description
-----------

This role will in a configurable manner:

- install Elasticsearch
- configure Elasticsearch
- create TLS certificates
- configure http and transport TLS
- set passwords for built-in users
- sync all the above information with the ansible controller for further use

Configuration happens via a yaml dict (`elastic_config`), thus every default
configuration performed by this role can be overridden by defining the
appropriate keys in `elastic_config`.

Requirements
------------

None

Dependencies
------------

None. Of course, you will need to have java installed on the target system.

Role Variables
--------------

Look at the [defaults/main.yml](defaults/main.yml) file for this roles variables and their
documentation.

By default, the role will simply install Elasticsearch and start Elasticsearch
as a master, data and ingest node.

Comparison with other roles
---------------------------

Before creating this role, I tried to use for my projects the following two
roles:

- https://github.com/geerlingguy/ansible-role-elasticsearch
- https://github.com/elastic/ansible-elasticsearch

However, they were not fit for my needs. The first one is too simple and any PRs
to add functionality will most likely be stuck in the PR queue for months if not
years. The second one is too messy for me with old and hard to read ansible
code, a lot of bulk from previous versions and confusing documentation.

Example Playbook
----------------

This is a minimal playbook to have elasticsearch installed as soon as possible,
with no certificates, for development purposes.

```yaml
- hosts: elastic-server
  roles:
    - nkakouros.elasticsearch
```

For a full example on how to configure and install a full ELK installation (from
where you can pick what is relevant for your use case) see the
[molecule/default/](molecule/default/) folder. In there, the
[prepare.yml](molecule/default/prepare.yml) file contains a playbook that will
install dependencies that this role will need. The
[playbook.yml](molecule/default/playbook.yml) file will contain a full and
complex example of how to use this role specifically.

License
-------

GPLv3

Author Information
------------------

Nikolaos Kakouros (nkak@kth.se)
