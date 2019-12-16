# rhgs_install
Necessary plays to deploy Red Hat Gluster Storage

## Introduction 
These set of plays allows the installation of RHGS on a defined set of hosts defined in the inventory file. Currently, the plays have been tested using RHN and not Satellite. 

To run the play, the following vars need to be set:

* rhn_user; as a valid RHN user name 
* rhn_pass; as the related password from the used RHN user name
* rhn_pool; as a pool id (check subscription-manager list --available)
* host; as a group of host defined in inventory file
* nfs; as [blank,yes] to deploy/or not NFS Ganesha 
* smb; as [blank,yes] to deploy/or not Gluster SMB
