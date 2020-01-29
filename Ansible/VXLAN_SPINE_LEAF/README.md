# vxlan_spine_leaf
Ansible repo for vxlan spine leaf [topology](./vxlan_spine_leaf_CL.pdf)

## How to use these playbooks.

The playbooks in this repo can be used to configure the underlay and overlay for a VXLAN spine leaf topology and add a L3 VNI tenant.

Cloan this repo in a location where you typically run ansible playbooks.

Run the playbooks in the following order.

```
# Configure OSPF underlay
ansible-playbook -i vxlan_hosts underlay.yaml

# Verify IP reachability between spines and leafs
ansible-playbook -i vxlan_hosts ping.yaml

# Configure Overlay on Spines
ansible-playbook -i vxlan_hosts overlay_spines.yaml

# Configure Overlay on Leafs
ansible-playbook -i vxlan_hosts overlay_leafs.yaml

# Add tenant 1 to Leaf1
ansible-playbook -i vxlan_hosts overlay_tenant1_leaf1.yaml

# Add tenant 1 to Leaf3
ansible-playbook -i vxlan_hosts overlay_tenant1_leaf3.yaml

# Add tenant 2 to Leaf1
ansible-playbook -i vxlan_hosts overlay_tenant2_leaf1.yaml

# Add tenant 2 to Leaf3
ansible-playbook -i vxlan_hosts overlay_tenant2_leaf3.yaml

# Configure host devices
ansible-playbook -i vxlan_hosts host_devices.yaml

# Display VXLAN Overlay Operational State
ansible-playbook -i vxlan_hosts vxlan_operational_state.yaml
```
Verify you can ping on host D (192.168.10.20) from host A (192.168.0.10)

```
Host_A# show ip int br 

IP Interface Status for VRF "default"(1)
Interface            IP Address      Interface Status
Eth1/1               192.168.0.10    protocol-up/link-up/admin-up       

Host_A# ping 192.168.10.20
PING 192.168.10.20 (192.168.10.20): 56 data bytes
64 bytes from 192.168.10.20: icmp_seq=0 ttl=252 time=23.969 ms
64 bytes from 192.168.10.20: icmp_seq=1 ttl=252 time=22.909 ms
64 bytes from 192.168.10.20: icmp_seq=2 ttl=252 time=22.329 ms
64 bytes from 192.168.10.20: icmp_seq=3 ttl=252 time=22.483 ms
64 bytes from 192.168.10.20: icmp_seq=4 ttl=252 time=21.338 ms
```
Verify you can ping on host C (192.168.0.20) from host B (192.168.0.10)
```
Host_B# show ip int br 
IP Interface Status for VRF "default"(1)
Interface            IP Address      Interface Status
Eth1/1               192.168.0.10    protocol-up/link-up/admin-up       
Host_B# 

Host_B# ping 192.168.0.20
PING 192.168.0.20 (192.168.0.20): 56 data bytes
64 bytes from 192.168.0.20: icmp_seq=0 ttl=254 time=21.744 ms
64 bytes from 192.168.0.20: icmp_seq=1 ttl=254 time=19.901 ms
64 bytes from 192.168.0.20: icmp_seq=2 ttl=254 time=21.521 ms
64 bytes from 192.168.0.20: icmp_seq=3 ttl=254 time=21.562 ms
64 bytes from 192.168.0.20: icmp_seq=4 ttl=254 time=21.415 ms
```
To clean up all of the overlay and underlay configs run the following playbook.

```
ansible-playbook -i vxlan_hosts cleanup.yaml
```



