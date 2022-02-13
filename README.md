Prepare environment
-------------------
```shell
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
cd ansible
```


Installing Ansible requirements
-------------------------------

```shell
$ ansible-galaxy install -r ansible/requirements.yml 
Starting galaxy role install process
- extracting rfc2_bootstrap to /home/akimrx/.ansible/roles/rfc2_bootstrap
- rfc2_bootstrap (1.0.0) was installed successfully
```

List installed Ansible roles and collections
--------------------------------------------

```shell
(venv) akimrx desktop ~/github/rfc2/rfc2_any_repo on  master! ⌚️ 11:11:32
$ ansible-galaxy list                               
# /home/akimrx/.ansible/roles
- rfc2_bootstrap, 1.0.0
```


Run playbook
------------

```shell
$ ansible-playbook -i inventory/example/hosts.yml playbooks/webservers.yml          

PLAY [webservers] *****************************************************************************************************************
Sunday 13 February 2022  11:20:42 +0300 (0:00:00.010)       0:00:00.010 ******* 
Sunday 13 February 2022  11:20:45 +0300 (0:00:02.791)       0:00:02.802 ******* 

TASK [rfc2_bootstrap : bootstrap - install base packages] ************************************************************************************************************************************
changed: [lb1.akimrx.cloud]
Sunday 13 February 2022  11:20:51 +0300 (0:00:06.048)       0:00:08.850 ******* 

TASK [rfc2_bootstrap : bootstrap - set timezone] ************************************************************************************************************************************
changed: [lb1.akimrx.cloud]
Sunday 13 February 2022  11:20:53 +0300 (0:00:01.550)       0:00:10.400 ******* 

TASK [rfc2_bootstrap : bootstrap - set hostname] ************************************************************************************************************************************
changed: [lb1.akimrx.cloud]

PLAY RECAP *************************************************************************************************************************
lb1.akimrx.cloud           : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Sunday 13 February 2022  11:20:55 +0300 (0:00:02.223)       0:00:12.624 ******* 
=============================================================================== 
rfc2_bootstrap : bootstrap - install base packages ----------------------------------------------------------------------------------------------------------------------------- 6.05s
Gathering Facts ------------------------------------------------------------------------------------------------------------- 2.79s
rfc2_bootstrap : bootstrap - set hostname ----------------------------------------------------------------------------------------------------------------------------- 2.22s
rfc2_bootstrap : bootstrap - set timezone ----------------------------------------------------------------------------------------------------------------------------- 1.55s
Playbook run took 0 days, 0 hours, 0 minutes, 12 seconds
```

Force update dependencies for version upgrade
---------------------------------------------
```shell
$ ansible-galaxy install -r requirements.yml --force
Starting galaxy role install process
- changing role rfc2_bootstrap from 1.0.0 to 1.0.1
- extracting rfc2_bootstrap to /home/akimrx/.ansible/roles/rfc2_bootstrap
- rfc2_bootstrap (1.0.1) was installed successfully
```

Restart the playbook after updating the dependencies
----------------------------------------------------
```shell
$ ansible-playbook -i inventory/example/hosts.yml playbooks/webservers.yml          

PLAY [webservers] *****************************************************************************************************
Sunday 13 February 2022  12:02:14 +0300 (0:00:00.010)       0:00:00.010 ******* 
Sunday 13 February 2022  12:02:17 +0300 (0:00:02.485)       0:00:02.496 ******* 
Sunday 13 February 2022  12:02:20 +0300 (0:00:03.137)       0:00:05.634 ******* 
Sunday 13 February 2022  12:02:21 +0300 (0:00:01.483)       0:00:07.117 ******* 
Sunday 13 February 2022  12:02:23 +0300 (0:00:01.903)       0:00:09.021 ******* 

TASK [rfc2_bootstrap : bootstrap - install NTP package] ***********************************************************************************************************************
changed: [lb1.akimrx.cloud]
Sunday 13 February 2022  12:02:33 +0300 (0:00:10.095)       0:00:19.117 ******* 

TASK [rfc2_bootstrap : bootstrap - configure NTP service] ***********************************************************************************************************************
changed: [lb1.akimrx.cloud]
Sunday 13 February 2022  12:02:36 +0300 (0:00:02.982)       0:00:22.099 ******* 

RUNNING HANDLER [rfc2_bootstrap : ntp - restart] ***********************************************************************************************************************
changed: [lb1.akimrx.cloud]

PLAY RECAP ************************************************************************************************************
lb1.akimrx.cloud           : ok=7    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

Sunday 13 February 2022  12:02:38 +0300 (0:00:01.950)       0:00:24.049 ******* 
=============================================================================== 
rfc2_bootstrap : bootstrap - install NTP package ------------------------------------------------------------------------------------------------------------- 10.10s
rfc2_bootstrap : bootstrap - install base packages -------------------------------------------------------------------------------------------------------------- 3.14s
rfc2_bootstrap : bootstrap - configure NTP service -------------------------------------------------------------------------------------------------------------- 2.98s
Gathering Facts ---------------------------------------------------------------------------------------------- 2.49s
rfc2_bootstrap : ntp - restart ------------------------------------------------------------------------------- 1.95s
rfc2_bootstrap : bootstrap - set hostname -------------------------------------------------------------------------------------------------------------- 1.90s
rfc2_bootstrap : bootstrap - set timezone -------------------------------------------------------------------- 1.48s
Playbook run took 0 days, 0 hours, 0 minutes, 24 seconds
```


Storing multiple versions of the same role
------------------------------------------

Example `requirements.yml`:
```yaml
---
- src: https://github.com/akimrx/rfc2_bootstrap
  version: 1.0.1
  name: bootstrap-1.0.1
- src: https://github.com/akimrx/rfc2_bootstrap
  version: 1.0.0
  name: bootstrap-1.0.0
```

Install dependencies:
```shell
$ ansible-galaxy install -r requirements.yml --force                                
Starting galaxy role install process
- extracting bootstrap-1.0.1 to /home/akimrx/.ansible/roles/bootstrap-1.0.1
- bootstrap-1.0.1 (1.0.1) was installed successfully
- extracting bootstrap-1.0.0 to /home/akimrx/.ansible/roles/bootstrap-1.0.0
- bootstrap-1.0.0 (1.0.0) was installed successfully
```

Show installed roles:
```shell
$ ansible-galaxy list                                                               
# /home/akimrx/.ansible/roles
- bootstrap-1.0.1, 1.0.1
- rfc2_bootstrap, 1.0.1
- bootstrap-1.0.0, 1.0.0
```


Example usage in playbooks:
```yaml
---
- hosts: servers1
  roles:
    - bootstrap-1.0.0

- hosts: servers2
  roles:
    - bootstrap-1.0.1
```