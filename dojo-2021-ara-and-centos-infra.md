# Keeping track of CentOS infrastructure deployments
## (with Ansible and ARA)
### David Moreau-Simard / Fabian Arrotin

May 2021 @ CentOS Dojo

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

## /whois dmsimard
##### dmsimard@redhat.com = ['ansible', 'ops', 'community', 'tooling', 'ci/cd']

## /whois arrfab
##### arrfab@centos.org = ['ops', 'infra', 'floor sweeper']


---
# Agenda

## ARA Records Ansible:
 * What it is (and isn't)
 * What it does
 * How it works
 * Getting started
---
# Agenda

## CentOS infrastructure
 * Why ARA
 * How we consume it
 * ConfigManagement SIG work

---
![ara-logo](img/ara-logo.png)

## What it is

- 5 year old 🎂
- Another recursive acronym
- A free and open source Ansible community project (GPLv3)
- Packaged on PyPi, for Fedora, CentOS, Debian and OpenSUSE
- Available on [DockerHub](https://hub.docker.com/r/recordsansible/ara-api) and [Quay.io](https://quay.io/repository/recordsansible/ara-api)

---
## What it isn't

- A tool for running playbooks
- A replacement for AWX (or Tower)
- A parser for your Ansible console output

---
## What it does

- Makes Ansible easier to understand and troubleshoot
- Records playbook results to a sqlite, mysql or postgresql database
- Provides an API for playbook, task and host results
- Provides a web reporting interface and a CLI to browse results

---
## How it works

![ara-recording-workflow](img/ara-recording-workflow.png)

---
class: center, middle

![ara-how](img/ara-how.png)

---
## How it works

About Ansible callback plugins...

```python
# https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/callback/__init__.py
# [...]
    def v2_on_any(self, *args, **kwargs):
    def v2_runner_on_failed(self, result, ignore_errors=False):
    def v2_runner_on_ok(self, result):
    def v2_runner_on_skipped(self, result):
    def v2_runner_on_unreachable(self, result):
    def v2_playbook_on_start(self, playbook):
    def v2_playbook_on_task_start(self, task, is_conditional):
    def v2_playbook_on_play_start(self, play):
    def v2_playbook_on_stats(self, stats):
    def v2_runner_item_on_ok(self, result):
    def v2_runner_item_on_failed(self, result):
    def v2_runner_item_on_skipped(self, result):
# [...]
```

---
# Getting started

With PyPi:

```bash
python3 -m pip install --user ansible ara[server]
export ANSIBLE_CALLBACK_PLUGINS=$(python3 -m ara.setup.callback_plugins)
ansible-playbook playbook.yml
ara playbook list
ara-manage runserver # -> http://127.0.0.1:8000
```

---
# Getting started

With CentOS 8 repositories:

```bash
dnf install -y centos-release-ansible-29
dnf install -y ansible ara-server
export ANSIBLE_CALLBACK_PLUGINS=$(python3 -m ara.setup.callback_plugins)
ansible-playbook playbook.yml
ara playbook list
ara-manage runserver # -> http://127.0.0.1:8000
```

---
![ara-logo](img/ara-logo.png)

# Links and community

Live demo: https://demo.recordsansible.org

More reading:
- **Website & Blog**: https://ara.recordsansible.org
- **Code**: https://github.com/ansible-community/ara
- **Documentation**: https://ara.readthedocs.io/

Stay up to date or come chat:
- **Twitter**: https://twitter.com/RecordsAnsible
- **IRC**: #ara on freenode
- **Slack**: Invite in the README (*bridged to IRC!*)

---
# CentOS Infra 
## How it started

* Need to have central ansible station
* Good to have reports (// foreman dashboard for puppet execution)

---
# CentOS ConfigManagement SIG work
 * rebuild ara (client part)
--

 * rebuild needed pkgs non in EPEL8
--

 * rebuild ara with server support

---
# CentOS Infra
## We started with container
https://github.com/ansible-community/ara/blob/master/contrib/container-images/centos8-pypi.sh

```bash
podman pull quay.io/recordsansible/ara-api:centos8-pypi-latest

```
More info: https://github.com/CentOS/ansible-role-ansible-host/blob/master/defaults/main.yml#L62

---
# CentOS Infra
## rpm variant (now that we have it)

```bash
sudo dnf install -y centos-release-ansible-29
sudo dnf install -y ansible ara python3-ara-server python3-gunicorn
```

---
# CentOS Infra
## systemd unit file (not provided by ara pkg)

```bash
[Unit]
Description=ARA api service
DefaultDependencies=no
After=network.target

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=ara
Group=ara
WorkingDirectory=/home/ara/.ara
ExecStart=/usr/bin/gunicorn --workers={{ ansible_ara_workers }}  ara.server.wsgi
ExecStop=/bin/kill -s TERM $MAINPID
```
---
# CentOS infra
## KiSS (sqlite)

```bash
-rw-r--r--.  1 ara  ara  5.2G May 10 12:55 ara.sqlite
```
---
class: left, top
# CentOS Infra
.left[![ara-centos](img/centos-ara-host.png)]

---
class: center, middle
# Thank you !
## Any questions ?
