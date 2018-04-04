# nsxt-baremetal-SERVER
Internal only prototype for running baremetal server with NSX-T
<br/>I have added my hosts file just for a reference. 

```
[nsxmanager]
nsxmanager	ip=10.29.15.210	password=VMware1!	parent_vifs_ls=d3afd40d-1bb0-42dc-b14b-7a8b3493e2f0
```

ip is the ip address of my NSX Manager
password is the admin password
parent_vifs_ls is the nsx uuid of the logical switch where we want the patch to be connected to (nodes parent VIFs).


<br/>
Use at your own risk :)

