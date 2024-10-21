# Dockerfile linters and more

Linting is a crucial step in development, since it can bring many insights and best practices that can help us avoid commiting horrible mistakes.

The first tool we've tested and found was **Hadolint**, which is not included now since it was apparently rewritten in **Semgrep** and can be easily replaced by a rule. Either way, we suggest you try Hadolint for itself to double check.

## Semgrep

Here you can run Selected rules from Hadolint:

```bash

semgrep --config "p/dockerfile" Dockerfile
```

You can also use some more rules like the following ones!

Security checks for docker-compose configuration files:

```bash
semgrep --config "p/docker-compose" 
```

Security checks for kubernetes configuration files:

```bash
semgrep --config "p/kubernetes"
```

Since you've already seen many examples of this tool, I'm not going to show many examples, nor output, and skip to the next one. 

## Dockle

This is an amazing and widely used linter to scan images, but it only scans images. In order to scan you either need to provide an image name and tag, or save it to a tar file and then scan it locally.

Here's an example from their repository:
```bash
wanderer@trg DevSecOps-toolkit $ dockle goodwithtech/test-image:v1
FATAL	- CIS-DI-0009: Use COPY instead of ADD in Dockerfile
	* Use COPY : /bin/sh -c #(nop) ADD file:81c0a803075715d1a6b4f75a29f8a01b21cc170cfc1bff6702317d1be2fe71a3 in /app/credentials.json
FATAL	- CIS-DI-0010: Do not store credential in environment variables/files
	* Suspicious filename found : app/credentials.json (You can suppress it with "-af credentials.json")
	* Suspicious ENV key found : MYSQL_PASSWD on /bin/sh -c #(nop)  ENV MYSQL_PASSWD=password (You can suppress it with --accept-key)
FATAL	- DKL-DI-0005: Clear apt-get caches
	* Use 'rm -rf /var/lib/apt/lists' after 'apt-get install|update' : /bin/sh -c apt-get update && apt-get install -y git
FATAL	- DKL-LI-0001: Avoid empty password
	* No password user found! username : nopasswd
WARN	- CIS-DI-0001: Create a user for the container
	* Last user should not be root
INFO	- CIS-DI-0005: Enable Content trust for Docker
	* export DOCKER_CONTENT_TRUST=1 before docker pull/build
INFO	- CIS-DI-0006: Add HEALTHCHECK instruction to the container image
```

Scan an image file. Note you won't be able to use docker inside our container since we don't have installed docker in docker. But you can save it from your host, and transfer it in any way you desire.

```bash
theredguild@home $ docker save python:3.4-alpine -o python.tar
theredguild@home $ dockle --input python.tar
DevSecOps-toolkit on workshop-minimal [$]
❯ docker save devsecops-toolkit:latest -o devsecops.tar
❯ dockle --input devsecops.tar
FATAL	- DKL-DI-0001: Avoid sudo command
	* Avoid sudo in container : RUN |26 USERNAME=wanderer GROUPNAME=trg USER_UID=1000 USER_GID=1000 T_2MS=3.10.0 T_CHECKOV=3.2.267 T_CLAIR=4.8.0 T_CLOUDSPLAINING=0.7.0 T_DEPCHECK=10.0.4 T_DETECT_SECRETS=1.5.0 T_DOCKLE=0.4.14 T_GITXRAY=1.0.16 T_GITLEAKS=8.21.0 T_GRYPE=0.82.1 T_HADOLINT=2.12.0 T_KICS=2.1.3 T_LEGITIFY=1.0.11 T_NODEJSSCAN=3.7 T_RETIRE=5.2.4 T_SCOUTSUITE=5.14.0 T_SEMGREP=1.91.0 T_SNYK=1.1293.1 T_TRIVY=0.56.2 T_TRUFFLEHOG=3.82.8 T_DEPSCAN=5.2.6 T_OCTOSCAN=0.1.1 /bin/sh -c apt-get update && apt-get install -y     curl     wget     git     build-essential     python3     python3-venv     python3-dev     python3-pip     gnupg     dirmngr     ca-certificates     libssl-dev     zlib1g-dev     libbz2-dev     libreadline-dev     libsqlite3-dev     libffi-dev     liblzma-dev     zsh     pipx     sudo     make     vim     unzip     default-jre     yarn     fdupes     && rm -rf /var/lib/apt/lists/* # buildkit

(many like these because I used too many sudos apparently)

INFO	- CIS-DI-0005: Enable Content trust for Docker
	* export DOCKER_CONTENT_TRUST=1 before docker pull/build
INFO	- CIS-DI-0006: Add HEALTHCHECK instruction to the container image
	* not found HEALTHCHECK statement
INFO	- CIS-DI-0008: Confirm safety of setuid/setgid files
	* setuid file: urwxr-xr-x usr/bin/newgrp
	* setgid file: grwxr-xr-x usr/bin/chage
	* setgid file: grwxr-xr-x usr/bin/expiry
	* setuid file: urwxr-xr-x usr/bin/chsh
	* setuid file: urwxr-xr-x usr/bin/umount
	* setuid file: urwxr-xr-x usr/bin/sudo
	* setuid file: urwxr-xr-x usr/bin/mount
	* setuid file: urwxr-xr-x usr/lib/openssh/ssh-keysign
	* setgid file: grwxr-xr-x usr/sbin/unix_chkpwd
	* setuid file: urwxr-xr-x usr/bin/passwd
	* setuid file: urwxr-xr-x usr/bin/su
	* setuid file: urwxr-xr-x usr/bin/gpasswd
	* setuid file: urwxr-xr-x usr/bin/chfn
	* setgid file: grwxr-xr-x usr/bin/ssh-agent
	* setuid file: urwxr-xr-- usr/lib/dbus-1.0/dbus-daemon-launch-helper
INFO	- DKL-LI-0003: Only put necessary files
	* Suspicious directory : home/wanderer/.local/pipx/.cache
	* Suspicious directory : home/wanderer/.asdf/plugins/nodejs/.git
	* Suspicious directory : home/wanderer/.asdf/plugins/golang/.git
	* Suspicious directory : home/wanderer/.asdf/.git
	* Suspicious directory : home/wanderer/.asdf/plugins/nodejs/.node-build/.git
	* unnecessary file : home/wanderer/.asdf/installs/golang/1.23.2/go/src/crypto/internal/boring/Dockerfile
	* Suspicious directory : src/gh-workflow-auditor/.git
	* Suspicious directory : home/wanderer/.npm
	* unnecessary file : home/wanderer/.asdf/installs/golang/1.23.2/go/src/crypto/internal/nistec/fiat/Dockerfile
```
