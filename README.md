# nsxt-containers-baremetal
Internal only prototype for running kubernetes/Opensfhift/PCF PODs on baremetal with NSX-T
<br/>I have added my hosts file just for a reference. 
<br/>I use this Ansible hosts file for my Openshift deployments. The only addition for baremetal is:
```
[nsxmanager]
nsxmanager	ip=10.29.15.210	password=VMware1!	parent_vifs_ls=d3afd40d-1bb0-42dc-b14b-7a8b3493e2f0
```

ip is the ip address of my NSX Manager
password is the admin password
parent_vifs_ls is the nsx uuid of the logical switch where we want the patch to be connected to (nodes parent VIFs).


This playbook automates the following for all openshift or k8s nodes: <br/>

1. Executes the following commands: <br/>
```
ovs-vsctl add-br br-int
ovs-vsctl set-fail-mode br-int standalone
ip link add nsx-patch-outer type veth peer name nsx-patch-inner
ip link set dev nsx-patch-outer up
ip link set dev nsx-patch-inner up
ovs-vsctl add-port br-int nsx-patch-inner
ovs-vsctl add-port nsx-managed nsx-patch-outer
ovs-vsctl set Interface nsx-patch-inner ofport=1
ovs-vsctl set Interface nsx-patch-outer external-ids:iface-id={some_id}
ovs-vsctl set Interface nsx-patch-outer external-ids:attached-mac={some_mac}
ovs-vsctl set Interface nsx-patch-outer external-ids:iface-status=active
```
2. Send API call to NSX to create Logical Port with the {some_id} from above <br/>
3. Place init script to make the configuration above persistent <br/>

<br/>
Use at your own risk :)

