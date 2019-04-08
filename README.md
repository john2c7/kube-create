## EC2 Cluster Create
A starting point for automating cluster creation.

#### Setup

##### Install Ansible
```s#h
$ sudo pip install ansible (or setup your favorite virtualenv)
ensure you are running 2.4.1+ or later. Otherwise this fix is necessary: https://github.com/ansible/ansible/pull/30333
```

##### Set AWS Credentials
```sh
$ export AWS_ACCESS_KEY_ID=’AK123′
$ export AWS_SECRET_ACCESS_KEY=’abc123′
```
##### Configure ansible.cfg
```sh
$ cp ansible.cfg.default ansible.cfg && vim ansible.cfg
Set your pem path
```

##### First Run
Be sure to open up your aws dashboard and watch things work. If you see any other 'test-dev' instances in the ec2 dashboard you can terminiate them, but duplicates will not be made if you do not and all shoudl still be well.
```sh
$ ansible-playbook -i hosts ec2-cluster-create.yml
```
or with all debug output
```sh
$ ansible-playbook -i hosts ec2-cluster-create.yml -vvv
```
Once complete you should see something like this:
```
PLAY RECAP ****************************************************************************
172.18.100.103             : ok=3    changed=3    unreachable=0    failed=0
172.18.110.151             : ok=1    changed=1    unreachable=0    failed=0
172.18.110.173             : ok=1    changed=1    unreachable=0    failed=0
172.18.110.222             : ok=1    changed=1    unreachable=0    failed=0
172.18.70.40               : ok=3    changed=3    unreachable=0    failed=0
172.18.80.246              : ok=3    changed=3    unreachable=0    failed=0
172.18.90.96               : ok=3    changed=3    unreachable=0    failed=0
localhost                  : ok=19   changed=10   unreachable=0    failed=0
```
##### Directory Structure
From best practices <a target="_blank" href="http://docs.ansible.com/ansible/latest/playbooks_best_practices.html#alternative-directory-layout"> here</a>

#### Notes
######  Tag Bug
Ensure you are running 2.4.1+ or otherwise make this ansible core fix: https://github.com/ansible/ansible/pull/30333

###### Dynamic Inventory 
Managed by ec2.py
Read more <a target="_blank" href="http://docs.ansible.com/ansible/latest/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script">here</a>.<br>
Configuration is kept at  /ec2.ini.
Cmdline example to list all ec2 nodes.
```sh
$ ./ec2.py --list
```
_ec2.py is set to return "test-dev" tagged instances only_

_Since there is no teardown yet be sure to delete you instances ( they are xl instances by default. Search for tag 'test-dev'_
