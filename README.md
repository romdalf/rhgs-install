# rhgs_install
Plays to deploy Red Hat Gluster Storage

## Introduction 
These set of plays allows the installation of RHGS on a defined set of hosts defined in the inventory file. Currently, the plays have been tested using RHN and not Satellite. 

To run the play, the following vars need to be set:

* rhn_user; as a valid RHN user name 
* rhn_pass; as the related password from the used RHN user name
* rhn_pool; as a pool id (check subscription-manager list --available)
* host; as a group of host defined in inventory file
* nfs; as [blank,yes] to deploy/or not NFS Ganesha 
* smb; as [blank,yes] to deploy/or not Gluster SMB

Example of run:

```
$ cat inventory
[dev]
rhel7-01 ansible_host=192.168.122.157 
rhel7-02 ansible_host=192.168.122.18

$ ansible-playbook rhgs_setup.yml -i inventory -e "rhn_user=myrhnuser rh_pool=000000000000000000000000000 host=dev"

RHN Password: 

PLAY [dev] ***********************************************************************************************

TASK [roles/sysprep : Verify if host is already registered] **********************************************
fatal: [rhel7-02]: FAILED! => {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python"}, "changed": true, "cmd": "subscription-manager status", "delta": "0:00:05.464408", "end": "2019-12-16 11:12:31.932991", "msg": "non-zero return code", "rc": 1, "start": "2019-12-16 11:12:26.468583", "stderr": "", "stderr_lines": [], "stdout": "+-------------------------------------------+\n   System Status Details\n+-------------------------------------------+\nOverall Status: Invalid\n\nRed Hat Enterprise Linux Server:\n- Not supported by a valid subscription.\n\nSystem Purpose Status: Not Specified", "stdout_lines": ["+-------------------------------------------+", "   System Status Details", "+-------------------------------------------+", "Overall Status: Invalid", "", "Red Hat Enterprise Linux Server:", "- Not supported by a valid subscription.", "", "System Purpose Status: Not Specified"]}
...ignoring
changed: [rhel7-01]

TASK [roles/sysprep : debug] *****************************************************************************
ok: [rhel7-01] => {
    "msg": "Logging in as myrhnuser with pool 000000000000000000000000000"
}
ok: [rhel7-02] => {
    "msg": "Logging in as myrhnuser with pool 000000000000000000000000000"
}

TASK [roles/sysprep : Subscribe to RHN] ******************************************************************
skipping: [rhel7-01]
changed: [rhel7-02]

TASK [roles/sysprep : Disable Repositories] **************************************************************
changed: [rhel7-02]
changed: [rhel7-01]

TASK [roles/sysprep : Enable Repositories] ***************************************************************
changed: [rhel7-02]
changed: [rhel7-01]

TASK [roles/sysprep : Update system] *********************************************************************
ok: [rhel7-01]
changed: [rhel7-02]

PLAY [dev] ***********************************************************************************************

TASK [roles/rhgs_install : Install RHGS] *****************************************************************
ok: [rhel7-01]
changed: [rhel7-02]

TASK [roles/rhgs_install : Configure firewall (TCP) for RHGS node communication] *************************
changed: [rhel7-02] => (item=22)
ok: [rhel7-01] => (item=22)
changed: [rhel7-02] => (item=111)
ok: [rhel7-01] => (item=111)
changed: [rhel7-02] => (item=24007)
ok: [rhel7-01] => (item=24007)
ok: [rhel7-01] => (item=24009)
changed: [rhel7-02] => (item=24009)
ok: [rhel7-01] => (item=49152-49664)
changed: [rhel7-02] => (item=49152-49664)

TASK [roles/rhgs_install : Configure firewall (UDP) for RHGS node communication] *************************
ok: [rhel7-01] => (item=111)
changed: [rhel7-02] => (item=111)

PLAY [dev] ***********************************************************************************************

PLAY [dev] ***********************************************************************************************

PLAY RECAP ***********************************************************************************************
rhel7-01                   : ok=8    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
rhel7-02                   : ok=9    changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   
```