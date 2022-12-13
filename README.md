# Windows Virtual Machine Azure Breakdown
---
> ## A list of all you need for a VM:
> 1. resource group
>
> 
> 1. virtual network
> 1. subnet
> 1. a public IP
> 1. a Network Security Group (NSG) and rule (optional shown in this demo)
> 1. a network interface for ip configuration
> 1. a connection between NSG and the network interface
> 1. a storage account
> 1. a tls private key 
> 1. create linux vm resource 
---
---
---
> ## These are the resource **NECESSARY** to support communication with a virtual machine:
 1. Virtual Network and Subnets
 1. Network Interfaces
 1. IP addresses

---
---
---

>## 1. The Virtual Network:
Any physical computer has a network to enable communication with devices such as other computers.

Virtualised machines are the same, but in this context they need a virtual network as they themselves are virtual. With the v_net VMs can communicate with other computers, other VMs, virtual servers etc.

V_nets can be made before or as you create a VM.

>## 2. Network Interfaces (NIC):
We are talking about the interconnection between a virtual machine and a virtual network. Naturally VMs must have at least one or what was the point in creating a v_net in the first place.

The number of NICs depends on the size of your vm itself, find out more here:

``https://learn.microsoft.com/en-us/azure/virtual-machines/sizes``

>## But what are Subnets for?
We can divide up a v_net into multiple subnets for organisation and security. 

You choose how you want to split up your v_net in creating it - you also get to choose your address range (which should be available obviously).

Make sure your address range doesn't overlap if the v_net is connected to other on-prem networks or equivalently other v_nets. The IP addresses are private and can't be accessed from the internet, only from within the v_net itself.

If working in an organisation where someone else is responsible for internal networks, consult them before selecting an address space

Intuitive Logic for requirement for VM besides benefits alluded to above:

Each NIC in a VM is connected to one subnet in one virtual network. This is the train of logic

1. Without creating subnets withing virtual networks you can't have NICs for your VM
2. Without NIC you have no connection between your v_net and VM conceptually
3. By proxy you thus don't have a VM without Subnets

Whether or not NICs are connected to the same or different subnets, they are able to communicate with one another without extra configuration

>## IP addresses
**Public IP addresses** - Can be optionally assigned to an NIC. Used for inbound and outbound communication with the Internet (without NAT *network address translation*) as well as any other Azure resources NOT connected to a v_net
(*reminder: NAT maps private addresses to public ones before transferring information*)
>What can you assign Public IP addresses to:
1. VMs
2. Public Load Balancers

**Private IP addresses** - Used for any communication within a v_net. Can also be used for communication with the internet with NAT. There must be at least one private IP address assigned to a VM.

>What can you assign Private IP addresses to:
1. VMs
2. Internal Load Balancers
>## TLS Private Key? What for?
For linux you need a TLS key to allow a linux system to verify one's identity to subsequently establish a network connection to another system.

Naturally then, without it your v_net and NIC connected to a subnet will be toothless, none of it will be possible.

>## These are optional Resources that can be created with the VM:
1. Network Security Groups (NSGs)
2. Load Balancers 

>## How can you benefit with NSGs?
NSGs contain an Access Control List (ACL) rules allowing or denying network traffic to subnets, NICs or indeed both. 

This implies you can have many NSGs, for example NSGs associated with individual NICs connected to a subnet, or just the subnet itself, depending on the level of granularity you require.

ACL rules apply to any VMs in the NSG associated subnet.

NSGs have two sets of rules, inbound and outbound. Priorities for a rule must be unique within sets.

>Each "rule" has the following properties:
- Protocol
- Source and Destination Port Ranges
- Address Prefixes
- Direction of Traffic
- Priority
- Access Type

>## How can you benefit from Load Balancers:
This is a big one, load balancers are an enabler of high availability (HA) as well as network performance to applications.

It can balance incoming Internet traffic TO VMs or indeed balance traffic BETWEEN VMs in a v_net.

>It does this by **mapping incoming and outgoing traffic between**:

1. The public IP address and the load balancer port
2. The private IP address and the VM port.

