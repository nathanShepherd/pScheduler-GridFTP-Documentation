# pScheduler-GridFTP-Documentation
How to use pScheduler and an ansible script to quickly deploy a GridFTP Server and conduct file transfers. These instructions have been verified on minimal images of CentOS 7. 

This software is a plugin for the pScheduler project that adds a disk-to-disk feature to the testing suite. More information and an alternative installation process can be found on the [pScheduler Wiki](https://github.com/perfsonar/project/wiki/pScheduler-Disk-to-Disk-Plugin-Installation).

## 1. Prepare System for [pScheduler](https://github.com/perfsonar/pscheduler/wiki/Development-and-Test-System)
Curl the sytem-prep scripts from pscheduler master branch.
```
sudo curl -s -O https://raw.githubusercontent.com/perfsonar/pscheduler/master/scripts/system-prep
```
For VirtualBox guests on Linux or OS X where you wish to have your account and home directory available
-  Edit system-prep and uncomment and configure the four environment variables at the top.

Execute the system-prep script.
```
sudo sh ./system-prep
```

## 2. Clone Ansible-Playbook to install Globus and setup a GridFTP Endpoint
More detailed instructions are included in the Ansible Playbook's [README.md](https://github.com/nathanShepherd/Playbook-setup-globus-server). This step can be skipped if globus-url-copy is already installed on the machine.
```
sudo yum install ansible git
git clone https://github.com/nathanShepherd/Playbook-setup-globus-server.git
cd Playbook-setup-globus-server
ansible-playbook main.yml --user root --ask-pass
```
The ansible-playbook now will install the required dependencies for globus-url-copy, this may take a few minutes.

Then, setup your local globus server and enter your Globus ID and Password at the prompt.
```
sudo globus-connect-server-setup
cd ..
```

## 3. Clone pScheduler / GridFTP branch and make from source as root
```
git clone https://github.com/perfsonar/pscheduler.git --branch issue-155
cd pscheduler
sudo make
```

After completing step three you will have installed pscheduler and globus-url-copy. Test your installation with the following command:

## Finally, use pscheduler to conduct a disk-to-disk test using the Globus tool
```
pscheduler task --tool globus disk-to-disk \
--source ftp://speedtest.tele2.net/1KB.zip \
--dest file:///tmp/test.out \
--timeout PT3S
```

NOTE: This test will fail if the file /tmp/test.out exists before the test runs.

### UPDATE 01/10/2019
Conduct a GridFTP disk-to-disk test with multiple parallel streams using the option --parallel
```
pscheduler task --tool globus disk-to-disk --source gsiftp://sunn-dtn.es.net:2811/data1/10M.dat --dest file:///tmp/test.out --parallel 3 --timeout PT10S
```

Under the hood, pscheduler is running the following command:
```
globus-url-copy -vb ftp://speedtest.tele2.net/1KB.zip file:///tmp/test.out
```


