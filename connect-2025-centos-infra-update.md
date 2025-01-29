template: title

  
# CentOS Connect / January 2025
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
 * 2024 review 
 * 2025 roadmap
 * Q&A

---
# Infra SIG role

 * Supports CentOS Stream infra (and builds delivery)
 * Supports community infra:
  * CBS koji builders (SIGs)
  * mirror network
  * Website / mailing-lists / CI

---
# Reminder

![infra-lifecycle](img/devops-loop.png)

---
# Reminder

 * Design
 * Build / Deploy
 * Maintain / Operate
 * Decommission

---
# Maintenance

![maintenance](img/maintenance.jpg)
#### often underrated [TM]

---
# 2024 review (public tracker)

![tickets](img/centos-infra-2024-opened-tickets.png)

---
# 2024 review (public tracker)

![tickets](img/centos-infra-2024-opened-closed-tickets.png)


---
# Maintain / Operate
## Community Build System (cbs)

  - koji update to 1.34
  - (reinstalled builders to el9)
  - tickets for tags
  - onboarded c10s (and epel10), including rpmautospec
  - newer/faster/better/stronger aarch64 build hosts
---
# Maintain / Operate
## c7/c8s EOL

  - migrated ansible host[s] to el9 (ansible-core 2.14 for long time)
  - chassis workload split (out of warranty , old ocp / ci infra)

---
# Maintain / Operate
## c10s onboarding

  - c10s tags in cbs
  - debuginfod : ensuring c10s support
  - duffy / CI : adding c10s in pools 

---
# Maintain / Operate
## BAU

  - backups
  - mirror cdn
  - relationship with sponsors

---
# Maintain / Operate
## sponsors

  - Kudos to 19 sponsors in 2024 for refreshed hardware ! (https://blog.centos.org/2024/09/thank-you-all-centos-sponsors/)

---
# Design / Build / Deploy

 * mailman3 (collab) for https://lists.centos.org

???
Michel, Michal, Jonathan and epel folks

--

 * website2 + docs.centos.org revamp (Artwork + docs SIGs collab)

--

 * s390x arch onboarding for CBS community builders


---
# Decommission

  - torrent trackers
  - forums (redirecting to fedora discourse)

---
# Roadmap 2025

  - Onboarding new team member ! (welcome Greg !)
  - BAU
    - Stream internal support
    - sponsors/backups/mirrors/cbs/etc ...
  - dc move[s] (impacted two times) #1579
    - hybrid (more aws usage)
    - more sinergy with Fedora infra ...
    - secureboot solution for SIGs (no ticket yet ?)
  - pagure => gitlab migration ? (no ticket yet)
    - investigation
  - migrating el8 to el9/el10 (nftables #1526)

---
class: center, middle
# Q&A 

---
class: center, middle
![thanks](img/thanks.png)
###### Images under [Pixabay License](https://pixabay.com/service/terms/)

