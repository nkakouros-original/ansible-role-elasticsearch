ansible-role-elasticsearch
=========

Installs and configures Elasticsearch

This role will:

- install Elasticsearch
- configure Elasticsearch
- create TLS certificates
- set passwords for built-in users

Requirements
------------

None

Role Variables
--------------

Look at the defaults/main.yml file for this roles variables and their
documentation.

By default, the role will install Elasticsearch, create and download
certificates, set random passwords for built-in users and start Elasticsearch as
a master, data and ingest node.

Dependencies
------------

None

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

```yaml
- hosts: elastic-server
  roles:
    - reallyenglish.apt-repo
    - geerlingguy.java
    - nkakouros.elasticsearch  # this role
  vars:
    apt_repo_to_add:
      - ppa:webupd8team/java
    java_packages:
      - openjdk-8-jre
    elastic_cluster_name: watchmen
    elastic_node_name: nite-owl
    elastic_jvm_extra_config: |
      -Des.enforce.bootstrap.checks=true
    elastic_certificates_config:
      instances:
        - name: all
    elastic_certificate_download_dir: ~/Projects/elastic-server/
    elastic_certificate_file:
      "{{ elastic_certificate_download_dir }}/all/all.p12"
    elastic_certificates_password: 'secret-pass'
    elastic_builtin_users_password_file:
      "{{ elastic_certificate_download_dir }}/elastic_passwords"
    elastic_transport_host: _site_
    elastic_config:
      xpack:
        security:
          enabled: true
          hide_settings: 'xpack.security.authc.realms.native.*'
          authc:
            accept_default_password: false
            realms:
              native:
                native1:
                  enabled: true
                  order: 0
          transport:
            ssl:
              enabled: true
              verification_mode: certificate
              keystore:
                path: "{{ elastic_certificate_path }}"  # role variable from `defaults.yml`
              truststore:
                path: "{{ elastic_certificate_path }}"
          http:
            ssl:
              enabled: true
              verification_mode: certificate
              keystore:
                path: "{{ elastic_certificate_path }}"
              truststore:
                path: "{{ elastic_certificate_path }}"

```

License
-------

GPLv3

Author Information
------------------

Nikolaos Kakouros (nkak@kth.se)
