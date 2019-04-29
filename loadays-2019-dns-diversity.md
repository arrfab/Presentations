
# OSS diversity is good
## (aka why we run various DNS servers flavors in CentOS.org infra)
### Fabian Arrotin
#### arrfab@centos.org, @arrfab

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

# /whois arrfab 
#### (tempted to answer : "same answer as yesterday")
---
# Agenda

## CentOS.org Infra DNS usage:
 * Bind
 * PowerDNS
 * Unbound
 * Dnsmasq

#### Ansible Roles to deploy/manage those
---
# Bind
## Legacy
```bash
whois centos.org|egrep "Creation Date"
Creation Date: 2003-12-04T12:28:30Z
```

---
# Bind
## Started small , only one zone : centos.org
* Few donated machines
* Small needs
* Small changes
* Managed by Puppet/SVN

---
# Bind (today)
```bash
[arrfab@ns1 ~]$ rpm -q bind-chroot
bind-chroot-9.9.4-73.el7_6.x86_64
```
* Several zones
* Ansible controlled
* Delegation for some Wildcards
  * (example) apps.ci.centos.org (Openshift cluster for CI)
---
# Bind
## Specific delegation for ACME protocol
### LetsEncrypt use-case

---
# PowerDNS 
## first usage (time machine !)
### mirror.centos.org case
* More and more nodes donated to Project
* All around the world
* How to avoid just RR and use GeoIP ?

???
Think about that in ~2003/2004 .. and bind
We need something that would let us redirect to a pool transparently

---
# PowerDNS
Delegation of some host/role to pdns auth servers
```bash
dig @ns1.centos.org mirror.centos.org 

; <<>> DiG 9.9.4-RedHat-9.9.4-73.el7_6 <<>> @ns1.centos.org mirror.centos.org
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 22916
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 3, ADDITIONAL: 4
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;mirror.centos.org.		IN	A

;; AUTHORITY SECTION:
mirror.centos.org.	600	IN	NS	pdns1.centos.org.
mirror.centos.org.	600	IN	NS	pdns2.centos.org.
mirror.centos.org.	600	IN	NS	pdns3.centos.org.

```
---
# PowerDNS
### Entered PowerDNS with Pipe backend ! 
(https://doc.powerdns.com/md/authoritative/backend-pipe/)
```
At that time perl based backend (based on the backend.pl example)
```
???
Permitted us to redirect to "nearest" mirror.centos.org node in country/continent
manual lists/array in perl script, ipv4 only, so issue when typo during svn/puppet time
no automation to add/remove from monitoring, so all manual changes through puppet

---
# PowerDNS today (2019)
### Still Pipe, but switched to python :
 * Central DB (also controlled by Zabbix/monitoring)
 * Add/remove hosts from DB (one "hostname" = one column in DB with true/false)
 * Reproduces backend.json / GPG encrypt it
 * PowerDNS pipe detects new backend.json, reload

---
# PowerDNS backend.json
```json
{
    "mirror": {
        "AF": {
            "ipv4": [], 
            "ipv6": []
        }, 
        "NA": {
            "ipv4": [
                "192.168.1.1", 
                "192.168.2.2"
            ], 
            "ipv6": [
                "::2", 
                "::3"
            ]
        }, 
      } 
}
```
.footnote[https://github.com/CentOS/pdns-custom-geoip-backend]
---
# Unbound

---
# Dnsmasq


---
# Ansible roles for CentOS.org infra:
* https://github.com/CentOS/ansible-role-bind
* https://github.com/CentOS/ansible-role-pdns-pipe

---
class: center, middle
# Q&A

