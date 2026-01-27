template: title

  
# CentOS Connect / January 2026
## Infra SIG update
### Fabian Arrotin 

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

# /whois arrfab 

### ['Floor Sweeper', 'positive attitude bringer', 'Hybrid Clown Strategist', 'Old School thinker', 'non LLM/AI assisted real human'] @centos.org

???
IA slide FTW !

---
# Agenda

## CentOS Infra SIG

 * Infra SIG role
 * 2025 review 
 * 2026 roadmap
 * Q&A

---
# Infra SIG role

 * Supports CentOS Stream infra (and builds delivery)
 * Supports community infra:
  * CBS koji builders (SIGs)
  * CI infra (Duffy / Openshift)
  * Mirrors and CDN network
  * Website / mailing-lists 

---
# 2025 review

 * Maintenance (upgrade, update, decommission, replace)
 * New deliverables (ScOS)
 * Adding Stream10/el10 support in some of our roles (including switch from iptables to nftables)
 * DC Move !
  * IAD2 => RDU3 (Fedora but some CentOS Machines too)
  * RDU2 => RDU3 (CentOS)
  * Bye bye Pagure ! (git.centos.org), all now on gitlab.com/CentOS

---
## 2025 review
### Welcome ScOS on the mirror network

Stream CoreOS (ScOS) available at https://cloud.centos.org/centos/scos/ 
 * available "streams" : 9 and 10
 * behind also migrated cloud.centos.org (AWS Cloudfront CDN)
 * ... advantage of a cloud SIG meetup here at connect 2025 ! :)

---
# 2025 review - DC move[s]

![moving](img/moving.png)

---
# 2025 review - DC move
## IAD2 to RDU3
Collab with Fedora/Red Hat folks
 * dependency on IPA/FAS
 * migrated ipsilon to RHEL9 (from RHEL8)
 * other small proxies in front of accounts.centos.org

---
#### RDU2 to RDU3 : Embracing Hybrid Cloud strategy
![hybrid-cloud](img/hybrid-cloud.png)

---
# 2025 review - DC move
## RDU2 to RDU3
Overview:
  * Tier 0 mirror network head
  * Community Build System infra (aka cbs)
    * hub, builders, signing
  * Various proxies and web presence
  * mailman3 / MX servers
  * whole CI infra (see dedicated slide after) 

---

![cbs](img/cbs-cloud-vpc.png)


---
![ci](img/duffy-aws.drawio.png)

---
# 2025 review - Gitlab migration

 * archived all git.centos.org/rpms to gitlab.com/centOS/archives/git.centos.org/rpms/ (read-only / aka inactive)
 * imported/migrated other projects (including issues tracker) under SIGs umbrella
   * infra tracker moved to https://gitlab.com/CentOS/infra/tracker/-/issues
   * sprint/iteration cadence board [available](https://gitlab.com/CentOS/infra/tracker/-/boards/9715492?iteration_id=Current) (Agile/Scrum setup)

---

# Roadmap 2026

  * DC move (the return !)
    * Stream infra (internal to RH but priority #1 - see issues.redhat.com for tracker)
--
  * ppc64le support back for duffy/ci

--
  * dedicated gitlab runners (also using Duffy machines)

--
  * (..... SecureBoot .... for SIGs ? - TBD)
--
  * riscv64 enablement 
    * CentOS Stream 10
    * Then available for SIGs through cbs.centos.org

--
![cbs](img/cbs-riscv-hosts.png)

---
class: center, middle
# Q&A 

---
class: center, middle
![thanks](img/thanks.png)
###### Images under [Pixabay License](https://pixabay.com/service/terms/)

