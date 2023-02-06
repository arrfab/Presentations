
# CentOS Dojo / February 2023
## CentOS infra services for SIGs
### Fabian Arrotin 

???
These are some notes presented in presenter mode and not on screen

---
class: center, middle, inverse

# /whois arrfab 
#### ['hybrid clown','floor sweeper'] @ centos.org

---
# Agenda

## CentOS Infra services for SIGs 
 * Git forge[s] (pagure and gitlab)
 * CBS koji build infra
 * Signing service
 * Mirror network / CDN
 * CI/testing infra (openshift and Duffy API)
 * Documentation hosting (mkdocs based)

---
# Hint / Disclaimer

Just visit https://sigs.centos.org/guide/ as starting point

---
# Git hosting

 * https://git.centos.org
 * SSO (OpenID / accounts.centos.org)
 * shared for el8/el9 upstream src.rpm

---
# Git hosting
 
 * https://gitlab.com/CentOS/
 * SSO (SAML / accounts.centos.org)
 * sub-{projects,groups} can be dedicated

---
# Lookaside
 
 * https://git.centos.org/sources
 * old layout (sha1sum based)
 * new layout (sha512sum based)

---
# Lookaside cache
## old layout (lookaside_upload)

```bash
package1/
└── c8s-sig_name-foo
    └── sha1sum_source_file
```

```bash
cat .package1.metadata file
d6616b89617914a0dd0fd5cfa06b0afc7a4541c4 SOURCES/package1-upstream-source-v1.0.tar.gz
```

---
# Lookaside cache
## new layout (lookaside_upload_sig)

```bash
package2/
└── source_file_name
    └── sha512sum_source_file_name
```

```bash
cat sources
SHA512 (package2-1.0.tar.gz) = f7cfbc956199b1a0342f321a0e4cb055d6ac2c784a8faace43cf80772f0de34b58ede1a02a558cd03e25c14938ac6e17b3746b1919c0bbc1f5b2955472577e4f

```

---
# CBS / Koji

  * dedicated for Special Interest Groups
  * supported architectures : [x86_64, aarch64, armhfp, ppc64, ppc64le]

--

  * roadmap : Images/Spins builds
    * Kiwi : working (waiting for signing service)
    * ImageBuilder => to be planned
 
---
# Signing service 

  * rpm gpg signature
  * (eventually ?) detached signature
    * would depend on new RFE for image building


---

background-image: url(img/CBS-SIGs-workflow.png)


---
# Mirror network / CDN
## buildlogs.centos.org

  * very small infra
  * .. but backed by CDN77
  * proposal: to be renamed ?

---
# Mirror network / CDN
## mirror.centos.org

 * to be decommissioned in June 2024
 * http://mirror.centos.org (sponsored nodes)
 * http://mirrorlist.centos.org (mirrorlist=)

---
# Mirror network / CDN
## mirror.stream.centos.org

 * Starting from CS9 (and beyond)
 * https://mirror.stream.centos.org (CloudFront based)
 * rsync://rsync.stream.centos.org (sponsored nodes)

---
# Mirror network
## Some stats (no CDN)

.left[![stats](img/centos-msync-jan-2023.png)]

---
# CI Testing

  * testing artifacts (rpm or eventually else)
  * hosted infra services:
    * OpenShift
    * Duffy 

---
background-image: url(img/duffy-aws.drawio.png)

---
# CI Testing
## Openshift

  * SIGs can ask for their namespace
  * tied to SSO (groups are synced) 
  * can use PVs (AWS EFS)
  * https://YOUR-SIG.apps.ocp.cloud.ci.centos.org

---
# CI Testing
## Duffy API

  * API endpoint https://duffy.ci.centos.org
  * provisions EC2/OpenNebula cloud images to you
  * reprovision in loop (ansible based)
  * no need for openshift (vice and versa)

---
# Documentation hosting

  * mkdocs based (Material for MkDocs variant)
  * git hosting independent
  * https://sigs.centos.org/<your_project>

---
class: center, middle
# Q&A 

---
class: center, middle
![thanks](img/thanks.png)
###### Images under [Pixabay License](https://pixabay.com/service/terms/)

