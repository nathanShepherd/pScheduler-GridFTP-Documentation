# pScheduler-GridFTP-Documentation
How to use pScheduler and an ansible script to quickly deploy a GridFTP Server and data transfers.

## Install pScheduler

## Clone Ansible-Playbook to initialize a Globus GridFTP Endpoint
Detailed instructions included in the Playbook's README.md
```
git clone https://github.com/nathanShepherd/Playbook-setup-globus-server.git
```

# Clone GridFTP branch 
```
git clone https://github.com/perfsonar/pscheduler.git --branch issue-155
```

# Enter pscheduler directory and conduct a test
```
pscheduler task --tool globus disk-to-disk \
--source ftp://speedtest.tele2.net/1KB.zip \
--dest /tmp/test.out  \
--timeout PT5S \
--dest-path /nill
```
