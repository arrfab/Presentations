
# CPE Tech Talk
## CentOS Infra monitoring with Zabbix
### Fabian Arrotin
#### arrfab@centos.org, @arrfab

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

# /whois arrfab 
#### ['ops','infra','floor sweeper'] @ centos.org
---
# Agenda

## CentOS.org monitoring
 * Zabbix quick overview
 * Why Zabbix for CentOS infra (how it started) ?
 * Current situation
 * Ansible automation around Zabbix (roles)

---
# Zabbix overview

 * OpenSource (already packaged as rpm)
 * no 'open-core' and 'enterprise' features
 * multiple DB support (MySQL in our case)
 * Easy to automate (puppet initially, ansible now)

---
# Zabbix overview

 * Main components :
  * zabbix server (storing in DB)
  * zabbix proxies (optional but recommended)
  * zabbix agent
 * 'All about metrics !'

---
# Zabbix overview
Other ways to collect metrics:

 * SNMP (like network switches)
 * IPMI (hardware level)
 * HTTP (gathering metrics like from prometheus)
 * Web monitoring (simulating http agent)
 * ... and more ... :)

---
# why Zabbix @ centos ?  

 * very small team (circa 2007 but still true somehow ...)
 * known by KB and myself (using at $work)
 * machines disappearing, need for alerting (sponsored infra)

---
# zabbix @ centos : current situation

 * Still following the LTS branch 
 * Actually switched to current 5.0.x LTS version
 * Rebuilt on https://cbs.centos.org for the following arches
  * el8/8-stream : x86_64, ppc64le, aarch64
  * el7 : x86_64, ppc64le, ppc64, aarch64, armhfp
 
---
# What to collect (default template)
What does an agent collect by default ? (template notion)
 * cpu
 * memory
 * automatic discovery (LLD):
  * network usage
  * disks/filesystems usage
 * processes
 * online/offline presence

---
# Adding more templates
Idea : start from common checks (baseline) and adding on top :
 * specific processes/tcp ports
 * web checks
 * "lego" style template (layered templates)
 

---
# Graphs, graphs everywhere ! [*]
 * Mirrorlist response time (group)
 * Aggregated graphs (msync network)




.footnote[.red.bold[*] yes, yes, demo coming]---
# Ansible automation
 * baseline role adds basic template at provision time
 * adds node to a group by default
 * Other ansible roles ? :
  * add 'other' template[s]
  * put node[s] in 'other' group[s]
---
# How so ? 
* Zabbix exposes API
* We (ab)use it through zabbix-cli
* Ansible delegation to host having zabbix-cli

---
# Ansible automation
Example with one role (httpd):
```bash
- block: 
    - name: Configuring agent in Zabbix server
      include_role:
        name: zabbix-server
        tasks_from: agent_config
      vars:
        zabbix_templates: "{{ httpd_zabbix_templates }}"
        zabbix_groups: "{{ httpd_zabbix_groups }}"
  delegate_to: "{{ zabbix_api_srv }}"  
  tags:
    - monitoring
  when: zabbix_api_srv is defined and zabbix_api_srv != 'None'
```
---
# Some links
 * https://www.zabbix.com
 * https://github.com/CentOS/ansible-role-zabbix-server
 * https://github.com/CentOS/ansible-role-zabbix-proxy
 * https://github.com/CentOS/ansible-role-zabbix-agent
 * https://github.com/unioslo/zabbix-cli
---
class: center, middle
# Q&A

