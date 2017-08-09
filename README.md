# ansible-dkim
Ansible role for opendkim with postfix configuration on Debian

### Example playbook
```yaml
---
- name: Installing and configuring postfix and opendkim
  hosts: mail
  remote_user: root
  vars_files: ['var_mail.yml']
  pre_tasks:
    - name: Ensuring /etc/opendkim/keys directory
      file: path=/etc/opendkim/keys state=directory 

    - name: Pushing DKIM private keys
      copy: src=keys/{{ item.key }} dest=/etc/opendkim/keys mode=0400
      with_items: "{{ dkim_domains }}"
  roles:
  - { role: ansible-dkim }

```
### `var_mail.yml`:


```yaml
---
admin_email: 'admin@domain.com'
postfix_myhostname: 'mailing.domain.com' # ehlo hostname
dkim_domains: 
  - domain: 'domain1.com'
    selector: 'domain1'
    key: 'domain1.private'
  - domain: 'domain2.fr'
    selector: 'domain2'
    key: 'domain2.private'

```
