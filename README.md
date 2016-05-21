Ansible Zimbra
=========

Install, configure and provision a Full Zimbra Server.

Requirements
------------

* CentOS 6, 7
* RHEL 6, 7
* Ubuntu 14.04
* Correctly configure DNS : now auto-configured using dnsmasq
* Correctly configued `/etc/hosts` file : now auto-configured by this ansible role

Role Variables
--------------

* `zimbra_download_url`, URL to download Zimbra
* `zimbra_file`, name of the downloaded file
* `zimbra_256sum_file`, `SHA256SUM` of the file
* `zimbra_password`, password for admin and everything
* `zimbra_default_domain`, default domain to create

**zimbra_domains**

* `name`, name of a domain
* `accounts`, Array of accounts
* `distribution_lists`, Array of Distribution Lists

**accounts**

* `name`, email of the account
* `password`, if empty the default pass is `12345678`

**distribution_lists**

* `name`, email of the Distribution List
* `members`, Array of email addresses of members
* `authorized_senders`, Array of domain accounts who can send email to the list

Example Playbook
----------------

```yaml
---
- hosts: all
  sudo: yes
  vars:
    zimbra_download_url: https://files.zimbra.com/downloads/8.6.0_GA/zcs-8.6.0_GA_1153.RHEL6_64.20141215151155.tgz
    zimbra_file: zcs-8.6.0_GA_1153.RHEL6_64.20141215151155
    zimbra_256sum_file: c2278e6632b9ca72487afdf24da2545238e325338090a9d8ad6e99b39593561c
    zimbra_password: Passw0rd
    zimbra_default_domain: 'mydom.com'

    zimbra_domains:
      - name: 'mydom.com'
        o: 'My Dom'
        accounts:
          - name: 'paul@mydom.com'
            zimbra_is_admin_account: TRUE
            password: Passw0rd

      - name: 'mydom2.com'
        o: 'My dom 2'

      - name: 'mydom3.com'
        o: 'My dom 3'
        accounts:
          - name: 'admin@mydom3.com'
            password: Passw0rd
            zimbra_is_domain_admin_account: TRUE

        distribution_lists:
          - name: 'user@mydom3.com'
            members:
              - '1@mydom4.com'
              - '2@mydom3.com'
            authorized_senders:
              - 'paul@mydom.com'

      - name: 'empty.com'
        o: 'My dom 4'

  roles:
    - role: zimbra
```

License
-------

MIT

Author Information
------------------

Based on ansible-zimbradev by pbruna

Modified by paulbsd
