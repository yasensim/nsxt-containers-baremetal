# nsxt-containers-baremetal
Internal only prototype for running kubernetes/Opensfhift/PCF PODs on baremetal with NSX-T
I have added my hosts file just for a reference. I use this Ansible hosts file for my Openshift deployments.
The only addition for baremetal is:

[nsxmanager]
nsxmanager	ip=10.29.15.210	password=VMware1!	parent_vifs_ls=d3afd40d-1bb0-42dc-b14b-7a8b3493e2f0


ip is the ip address of my NSX Manager
password is the admin password
parent_vifs_ls is the nsx uuid of the logical switch where we want to patch to be connected to.


This playbook automates the following for all openshift or k8s nodes: <br/>
ovs-vsctl add-br br-int <br/>
ovs-vsctl set-fail-mode br-int standalone <br/>
ip link add nsx-patch-outer type veth peer name nsx-patch-inner <br/>
ip link set nsx-patch-outer up <br/>
ip link set nsx-patch-inner up <br/>
ovs-vsctl add-port br-int nsx-patch-inner <br/>
ovs-vsctl add-port nsx-managed nsx-patch-outer <br/>
ovs-vsctl set Interface nsx-patch-inner ofport=1 <br/>
ovs-vsctl set Interface nsx-patch-outer external-ids:iface-id={some_id} <br/>
ovs-vsctl set Interface nsx-patch-outer external-ids:attached-mac={some_mac} <br/>
ovs-vsctl set Interface nsx-patch-outer external-ids:iface-status=active <br/>
Create Logical Port in NSX with the {some_id} from above <br/>
<br/>
Use at your own risk :)

