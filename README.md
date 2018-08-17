# pScheduler-GridFTP-Documentation
How to use pScheduler and an ansible script to quickly deploy a GridFTP Server and conduct file transfers. These instructions have been verified on minimal images of CentOS 7.

## 1. Prepare System for [pScheduler](https://github.com/perfsonar/pscheduler/wiki/Development-and-Test-System)
```
sudo curl -s -O https://raw.githubusercontent.com/perfsonar/pscheduler/master/scripts/system-prep
```
For VirtualBox guests on Linux or OS X where you wish to have your account and home directory available
-  Edit system-prep and uncomment and configure the four environment variables at the top.
```
sudo sh ./system-prep
```

## 2. Clone Ansible-Playbook to install Globus and setup a GridFTP Endpoint
Detailed instructions included in the Ansible Playbook's [README.md](https://github.com/nathanShepherd/Playbook-setup-globus-server). This step can be skipped if globus-url-copy is already installed.
```
sudo yum install ansible git
git clone https://github.com/nathanShepherd/Playbook-setup-globus-server.git
cd Playbook-setup-globus-server
ansible-playbook main.yml --user root --ask-pass
sudo globus-connect-server-setup
cd ..
```

## 3. Clone pScheduler / GridFTP branch and make from source as root
```
git clone https://github.com/perfsonar/pscheduler.git
cd pscheduler
sudo make
```

## 4. Build the Plugins to add them to the Server
```
cd pscheduler-test-disk-to-disk
sudo make cbic
cd ..

cd pscheduler-tool-globus
sudo make cbic
cd ..
```

## Finally, use pscheduler to conduct a disk-to-disk test using the Globus tool
```
pscheduler task --tool globus disk-to-disk \
--source ftp://speedtest.tele2.net/1KB.zip \
--dest file:///tmp/test.out \
--timeout PT3S
```
NOTE: This test will fail if the file /tmp/test.out exists before the test runs.

Under the hood, pscheduler is running the following command:
```
globus-url-copy -vb ftp://speedtest.tele2.net/1KB.zip file:///tmp/test.out
```


