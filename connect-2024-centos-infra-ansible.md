template: title

# CentOS Connect / February 2024
## Ansible usage in CentOS Infra
### Fabian Arrotin 

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

# /whois arrfab 
#### ['hybrid clown','floor sweeper'] @ centos.org

---
# Agenda

## Ansible usage in CentOS Infra

 * History
 * Ansible adoption
 * Modularity [TM]
 * Plugins/Reporting

---
# History
## A long time ago ...

 * manual + shell scripts / cluster-ssh
 * puppet + svn (~2007)
 * puppet 2.6/2.7 + git + foreman (ENC) (~2013) 
 
---
# Ansible adoption

 * initially for ad-hoc tasks 
 * ci.centos.org (bare-metal reinstall, no need for pki/post-install agent)
 * bikeshed discussion (I won !)
   * small inventory
   * all-in-one git project

---
# Puppet to Ansible migration

 * new code, open
 * each role is a git repo (github)
 * reusability in mind 
 * different environments => different inventories
 
---
# Inventory[ies]

 * specific per environment (Stream vs Staging vs public, vs CI, etc)
 * git repository[ies]
 * encrypted
  * git-crypt (easy git-filter using gpg)
  * ansible-vault


---
# Playbooks

  * shared for all env
  * https://github.com/centos/ansible-infra-playbooks
  * naming convention:
    * role-<name> => targeting ansible role
    * deploy-<name> => manual deployment (provisioning)
    * adhoc-<function> => ad-hoc task
---
# Roles layout

  * all on github.com/centos/ansible-role-***name***
  * different branches 
    * staging => staging env
    * master/main => prod env
  * default variables for *everything* (not reflecting truth / inventory)

---
# Roles layout
## Reusability

```yaml
- include_role:
    name: httpd
  vars:
    - httpd_tls: True
```
---
# Roles branch

> controlled by requirements.yml (different per env)
 
```yaml
roles:
  - src: https://github.com/CentOS/ansible-role-baseline
    name: baseline
    version: staging 
  - src: https://github.com/CentOS/ansible-role-bind
    name: bind
    version: staging 
```
---
# Updating Roles ? 

```bash
usage: ansible-galaxy role [-h] ROLE_ACTION ...
positional arguments:
  ROLE_ACTION
    init       Initialize new role with the base structure of a role.
    remove     Delete roles from roles_path.
    delete     Removes the role from Galaxy. It does not remove or alter the actual GitHub repository.
    list       Show the name and version of each role installed in the roles_path.
    search     Search the Galaxy database by tags, platforms, author and multiple keywords.
    import     Import a role into a galaxy server
    setup      Manage the integration between Galaxy and the given source.
    info       View more details about a specific role.
    install    Install role(s) from file(s), URL(s) or Ansible Galaxy
```

---
# Updating Roles ?

![wrong](img/wrong.png)

---
# Enters ansible-roles-ctl !

 * https://pypi.org/project/ansible-roles-ctl/
 * https://gitlab.com/osci/ansible-roles-ctl

```bash
usage: ansible-roles-ctl [-h] [--version] [--quiet] {status,install,update} ...
Manage roles installation
positional arguments:
  {status,install,update}
                        sub-command help
    status              inform about roles/collections installation status
    install             install roles/collections
    update              update roles/collections

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  --quiet, -q           less verbose display

```

---
# Collections usage (rant)

 * Is Ansible still "batteries included [TM] ?"
 * no way to have single pkg
 * split into ansible-core and *zillions* of collections

---
# Collections usage
> managed by ansible-roles-ctl from same requirements.yml file !

```yaml
collections:
  - name: ansible.posix
    type: git
    source: https://github.com/ansible-collections/ansible.posix.git
    version: 1.5.1
  - name: ansible.netcommon
    type: git
    source: https://github.com/ansible-collections/ansible.netcommon.git
    version: 2.0.0
  - name: community.general
    type: git
    source: https://github.com/ansible-collections/community.general.git
    version: 4.6.1
  - name: community.libvirt
    type: git
    source: https://github.com/ansible-collections/community.libvirt.git
    version: 1.0.2
  - name: community.mysql
    type: git
    source: https://github.com/ansible-collections/community.mysql.git
    version: 3.5.1

```

---
# Distributing files

  * Think PKI ("private should remain private" [TM])
  * host specific scripts (not in any role)
  * other needs
  * [pkistore,filestore]

---
# Ansible env workspace
> combining inventory + collections + playbooks + roles + pkistore/filestore

```bash
.
├── ansible.cfg
├── collections
│   └── ansible_collections
│       ├── <collection_names>
├── files -> playbooks/files
├── handlers -> playbooks/handlers
├── filestore
├── inventory
├── pkistore
├── playbooks
│   ├── files
│   ├── handlers
│   ├── requirements.yml
│   ├── vars
│   └── templates
├── roles
│   ├── <role-name>
└── templates -> playbooks/templates
└── vars -> playbooks/vars

```

---
# Plugins:  Mitogen (ansible on steroids)
> https://github.com/mitogen-hq/mitogen

```bash
{% if item.use_mitogen %}
strategy_plugins = {{ item.base_path }}/mitogen/ansible_mitogen/plugins/strategy
strategy = mitogen_linear
{% else %}
strategy = linear
{% endif %}
```


---
# Ansible speed-up 
What's consuming a *lot* of time for nothing during playbook execution ?

--

## Facts gathering ! 

--
from our ansible.cfg (per inventory)
```ini
# Adding some interesting stats
callback_whitelist = profile_tasks, timer
# Switch to smart so also using cached facts
gathering = smart
fact_caching = jsonfile
fact_caching_connection = .ansible_cache

```



---
class: left, top
# Plugins: Ara (Ansible Records Ansible - reporting)

.left[![ara-centos](img/centos-ara-host.png)]

---
# Plugins: Ara (Ansible Records Ansible - reporting)
> https://ara.recordsansible.org/

```bash
{% if ansible_use_ara %}
callback_plugins=/home/{{ item.owner }}/.local/lib/python{{ ansible_py_version }}/site-packages/ara/plugins/callback
action_plugins=/home/{{ item.owner }}/.local/lib/python{{ ansible_py_version }}/site-packages/ara/plugins/action
lookup_plugins=/home/{{ item.owner }}/.local/lib/python{{ ansible_py_version }}/site-packages/ara/plugins/lookup
[ara]
api_client = http
api_server = http://127.0.0.1:8000
default_labels = {{ item.name }}
{% endif %}


```

---
class: center, middle
# Q&A 

---
class: center, middle
![thanks](img/thanks.png)
###### Images under [Pixabay License](https://pixabay.com/service/terms/)

