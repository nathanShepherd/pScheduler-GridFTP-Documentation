# pScheduler-GridFTP-Documentation
How to use pScheduler and an ansible script to quickly deploy a GridFTP Server and conduct file transfers.

## Build and Install [pScheduler](https://github.com/perfsonar/pscheduler/wiki/Development-and-Test-System)

## Clone Ansible-Playbook to install Globus and setup a GridFTP Endpoint
Detailed instructions included in the [Playbook's README.md](https://github.com/nathanShepherd/Playbook-setup-globus-server). This step can be skipped if globus-url-copy is already installed.
```
git clone https://github.com/nathanShepherd/Playbook-setup-globus-server.git
```

## Clone pScheduler / GridFTP branch and replace local pscheduler branch
```
git clone https://github.com/perfsonar/pscheduler.git --branch issue-155
```

## Finally, use pscheduler to conduct a disk-to-disk test using the Globus tool
```
pscheduler task --tool globus disk-to-disk \
--source ftp://speedtest.tele2.net/1KB.zip \
--dest file:///tmp/test.out \
--timeout PT3S
```
Under the hood, pscheduler is running the following command:
```
globus-url-copy -vb ftp://speedtest.tele2.net/1KB.zip file:///tmp/test.out
```


