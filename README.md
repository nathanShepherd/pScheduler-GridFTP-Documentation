# pScheduler-GridFTP-Documentation
How to use pScheduler and an ansible script to quickly deploy a GridFTP Server and data transfers.

## Install pScheduler

## Clone Ansible-Playbook to initialize a Globus GridFTP Endpoint
Detailed instructions included in the Playbook's [README.md](https://github.com/nathanShepherd/Playbook-setup-globus-server)
```
git clone https://github.com/nathanShepherd/Playbook-setup-globus-server.git
```

## Clone GridFTP branch 
```
git clone https://github.com/perfsonar/pscheduler.git --branch issue-155
```

## Finally, enter pscheduler directory and download a test file
```
pscheduler task --tool globus disk-to-disk \
--source ftp://speedtest.tele2.net/1KB.zip \
--dest file:///tmp/test.out \
--timeout PT3S \
--dest-path /nill
```
Under the hood, pscheduler is running the following command:
```
globus-url-copy -vb ftp://speedtest.tele2.net/1KB.zip file:///tmp/test.out
```


