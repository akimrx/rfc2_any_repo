
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