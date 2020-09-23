# rhgs_install
The repository contains the necessary Ansible automation to deploy instances of GlusterFS.
Even if this Ansible plays have been written as generic as possible, this branch might have 
some specific modifications to match a specific customer deployment.

## Introduction 
These set of plays allows the installation of RHGS on a defined set of hosts.
To run the play, you will have to set up 2 files; the inventory and parameters files.

## Run the plays
To run the plays, the volume syntax needs to be setup:

```
$ ansible-playbook -i inventory -e "@parameters.yml" rhgs_setup.yml
```

### Inventory file
The file inventory contains the list of hosts under a label. All the active nodes need to be 
added to within there respective set. The current inventory has 3 environments defined as such:

```
[DPUB]
10.28.64.1
10.28.64.2
10.28.64.5
10.28.64.3
10.28.64.4
10.28.64.6

[EPUB]
10.36.64.1
10.36.64.2
10.36.64.5
10.36.64.3
10.36.64.4
10.36.64.6

[PPUB]
10.32.64.1
10.32.64.2
10.32.64.5
10.32.64.3
10.32.64.4
10.32.64.6
```

Note: the labels are fully customizable. Use whatever you like.

### Parameters file
The parameters file contains all the necessary variables to execute the plays with Ansible. This file can be taken as
reference for a Tower usage. 

```
---
## Inventory related settings
public: DPUB                            ## "public" defines the targeted set of nodes


## Provisioning related settings
gfsvol: "no"                        ## "gfsvol" switches on/off the creation of GlusterFS volume
                                    ## if "no" none of the below will be working
volname: ""                         ## "volname" defines the name for a new volume to be created
blockdevice: ""                     ## "blockdevice" defines the device name to be used for the LVM (ie. sdc)
lvm: "no"                           ## "lvm" switches on/off the creation of the LVM layers based on "blockdevice"
smbshare: ""    # enable/disable    ## "smbshare" switches on/off 



## Naming conventions settings
vgname: "gfsvg_{{ volname }}"       ## "vgname" defines the volume group name
thinpool: "gfstp_{{ volname }}"     ## "thinpool" defines the thin pool name
lvname: "gfslv_{{volname }}"        ## "lvname" defines the logical volume name



## Installation settings
## to use to install a new set of GlusteFS nodes
sysprep: "no"                       ## "sysprep" defines if the system needs to be subscribed 
install: "no"                       ## "install" defines if GlusterFS needs to be deployed
nfs: "no"                           ## "nfs" defines if NFS Ganesha needs to be deployed
smb: "no"                           ## "smb" defines if Samba needs to be deployed
smbad: "no"                         ## "smbad" defines if AD integration needs to be deployed
firewalld: "no"                     ## "firewalld" defines if firewalld needs to be configured

## Decommission settings
## delete the volume and all the LVM backend
## resulting in data deletion
zcleanup: "no"                      ## "zcleanup" defines if a volume needs to be decommissioned 

```

## Folders
Note that for sake of clarity a different approach regarding role folder structure has been taken.

```
roles
--rhgs_gfsvol                       ## provision a GlusterFS volume  
--rhgs_gfsvol_smb                   ## enable SMB for a GlusterFS volume
--rhgs_install                      ## install GlusterFS cluster
--rhgs_lvm                          ## provision LVM backend elements for GlusterFS volume
--rhgs_nfs                          ## install clustered NFS Ganesha
--rhgs_peering                      ## peer GlusterFS nodes together
--rhgs_smb                          ## install clustered SMB
--sysprep                           ## perform system preparation to deploy GlusterFS
--zcleanup                          ## decommission volume
```


