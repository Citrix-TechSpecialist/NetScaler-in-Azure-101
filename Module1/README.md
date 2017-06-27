# Module1: NetScaler Instantiation

## Module Overview

In this module you will login to your Azure account and instantiate a NetScaler VPX for us in Application Delivery for HTTPS Encryption Offload, TCP Proxy, Load Balancing and Content Switching, Rewrite, VPN, ICA Proxy, Web Logging, Content Switching, Content Filtering, Responder, HTML Injection, Web Interface on NS, and AppFlow.

Our latest software for NetScaler is 11.1, and the associated and most current documentation for NetScaler VPX on Azure is available on this link:[ https://docs.citrix.com/en-us/netscaler/11-1/deployingvpx/deploy-vpx-on-azure.html](https://docs.citrix.com/en-us/netscaler/11-1/deployingvpx/deploy-vpx-on-azure.html)

The NetScaler VPX virtual appliance is available as an image in the Microsoft Azure Marketplace. NetScaler VPX on Microsoft Azure Resource Manager (ARM) enables customers to leverage Azure cloud computing capabilities and use NetScaler load balancing and traffic management features for their business needs. You can deploy NetScaler VPX instances on Azure Resource Manager either as standalone instances or as high availability pairs in active-active or active-standby modes.

In ARM, a NetScaler VPX virtual machine (VM) resides in a virtual network. A virtual Network Interface Card (NIC) is created on each NetScaler VM. The network security group (NSG) configured in the virtual network is bound to the NIC, and together they control the traffic flowing into the VM and out of the VM. 

The NSG forwards the requests to the NetScaler VPX instance, and the VPX instance sends them to the servers. The response from a server follows the same path in reverse. The NSG can be configured to control a single VPX VM, or, with subnets and virtual networks, can control traffic in multiple VPX VM deployment. 

The NIC contains network configuration details such as the virtual network, subnets, internal IP address, and Public IP address.

While on ARM, it is good to know the following IP addresses used to access the VMs:
- Public IP (PIP) address is the Internet-facing IP address configured directly on the virtual NIC of the NetScaler VM. This allows you to directly access a VM from the external network without the need to configure inbound and outbound rules on the NSG.
- NetScaler IP (NSIP) address is internal IP address configured on the VM. It is non-routable.
- Virtual IP address (VIP) is configured by using the NSIP and a port number. Clients access NetScaler services through the PIP address, and when the request reaches the NIC of the NetScaler VPX VM or the Azure load balancer, the VIP gets translated to internal IP (NSIP) and internal port number.
- Internal IP address is the private internal IP address of the VM from the virtual network’s address space pool. This IP address cannot be reached from the external network. This IP address is by default dynamic unless you set it to static. Traffic from the internet is routed to this address according to the rules created on the NSG. The NSG works with the NIC to selectively send the right type of traffic to the right port on the NIC, which depends on the services configured on the VM. 

In this document, PIP, VIP, and Instance Level PIP (ILPIP) mean the same thing and are used interchangeably. 

The following figure shows how traffic flows from a client to a server through a NetScaler VPX instance provisioned in ARM.

[](./Images/img1)

# Module1: NetScaler Instantiation

## Module Overview

In this module you will login to your Azure account and instantiate a NetScaler VPX for us in Application Delivery for HTTPS Encryption Offload, TCP Proxy, Load Balancing and Content Switching, Rewrite, VPN, ICA Proxy, Web Logging, Content Switching, Content Filtering, Responder, HTML Injection, Web Interface on NS, and AppFlow.

Our latest software for NetScaler is 11.1, and the associated and most current documentation for NetScaler VPX on Azure is available on this link:[ https://docs.citrix.com/en-us/netscaler/11-1/deployingvpx/deploy-vpx-on-azure.html](https://docs.citrix.com/en-us/netscaler/11-1/deployingvpx/deploy-vpx-on-azure.html)

The NetScaler VPX virtual appliance is available as an image in the Microsoft Azure Marketplace. NetScaler VPX on Microsoft Azure Resource Manager (ARM) enables customers to leverage Azure cloud computing capabilities and use NetScaler load balancing and traffic management features for their business needs. You can deploy NetScaler VPX instances on Azure Resource Manager either as standalone instances or as high availability pairs in active-active or active-standby modes.

In ARM, a NetScaler VPX virtual machine (VM) resides in a virtual network. A virtual Network Interface Card (NIC) is created on each NetScaler VM. The network security group (NSG) configured in the virtual network is bound to the NIC, and together they control the traffic flowing into the VM and out of the VM. 

The NSG forwards the requests to the NetScaler VPX instance, and the VPX instance sends them to the servers. The response from a server follows the same path in reverse. The NSG can be configured to control a single VPX VM, or, with subnets and virtual networks, can control traffic in multiple VPX VM deployment. 

The NIC contains network configuration details such as the virtual network, subnets, internal IP address, and Public IP address.

While on ARM, it is good to know the following IP addresses used to access the VMs:

  * Public IP (PIP) address is the Internet-facing IP address configured directly on the virtual NIC of the NetScaler VM. This allows you to directly access a VM from the external network without the need to configure inbound and outbound rules on the NSG.
  
THIS IS A TEST!!!!

  * NetScaler IP (NSIP) address is internal IP address configured on the VM. It is non-routable.

  * Virtual IP address (VIP) is configured by using the NSIP and a port number. Clients access NetScaler services through the PIP address, and when the request reaches the NIC of the NetScaler VPX VM or the Azure load balancer, the VIP gets translated to internal IP (NSIP) and internal port number.
  - Internal IP address is the private internal IP address of the VM from the virtual network’s address space pool. This IP address cannot be reached from the external network. This IP address is by default dynamic unless you set it to static. Traffic from the internet is routed to this address according to the rules created on the NSG. The NSG works with the NIC to selectively send the right type of traffic to the right port on the NIC, which depends on the services configured on the VM. 

In this document, PIP, VIP, and Instance Level PIP (ILPIP) mean the same thing and are used interchangeably. 

The following figure shows how traffic flows from a client to a server through a NetScaler VPX instance provisioned in ARM.

![](./Module1/Images/img1)

##How NetScaler VPX Works on Azure

In an on-premises deployment, a NetScaler VPX instance requires, at least three IP addresses:
- Management IP address, called the NetScaler IP (NSIP) address
- Subnet IP (SNIP) address for communicating with the server farm
- Virtual server IP (VIP) address for accepting client requests

In an Azure deployment, only one IP address is assigned to an instance during provisioning through DHCP, and it is a private (internal) address.

Although you can provision a NetScaler VPX instance in Azure with multiple NICs, the multiple NIC feature has a few constraints that might make the instance unresponsive. For details about this constraint, see [Limitations and Usage Guidelines](http://docs.citrix.com/en-us/netscaler/10-5/vpx/deploy-vpx-on-azure.html)

To avoid this limitation, you can deploy NetScaler VPX instance in Azure with a single IP architecture, where the three IP functions of a NetScaler appliance are multiplexed onto one IP address. This single IP address uses different port numbers to function as the NSIP, SNIP, and VIP. 

The following image illustrates how a single IP address is used to perform the functions of NSIP, SNIP, and VIP.

![](./Images/img2)

### Note:
The single IP mode is available only in Azure deployments. This mode is not available for a NetScaler VPX instance on your premises, on AWS, or in in other type of deployment.

## Traffic Flow through Port Address Translation

In an Azure deployment, when you provision the NetScaler VPX instance as a virtual machine (VM), Azure assigns a Public IP address and an internal IP address (non-routable) to the NetScaler virtual machine. Inbound and Outbound rules are defined on the NSG for the NetScaler instance, along with a public port and a private port for each rule defined. The NetScaler instance listens on the internal IP address and private port. 

Any external request is received on the NetScaler VPX VM's virtual NIC. The NIC is bound to the NSG, which specifies the private IP and private port combination into which to translate the request's destination address and port (the Public IP address and port). ARM performs port address translation (PAT) to map the Public IP address and port to the internal IP address and private port of the NetScaler virtual machine, and forwards the traffic to the virtual machine. 

The following figure shows how Azure performs port address translation to direct traffic to the NetScaler internal IP address and private port. 

![](./Images/img3)

In this example, the Public IP address assigned to the VM is 140.x.x.x, and the internal IP address is 10.x.x.x. When the inbound and outbound rules are defined, public HTTP port 80 is defined as the port on which the client requests are received, and a corresponding private port, 10080, is defined as the port on which the NetScaler virtual machine listens. The client request is received on the Public IP address 140.x.x.x at port 80. Azure performs port address translation to map this address and port to internal IP address 10.x.x.x on private port 10080 and forwards the client request.

For information about port usage guidelines while, see [Port Usage Guidelines](http://docs.citrix.com/en-us/netscaler/10-5/vpx/deploy-vpx-on-azure.html).

For information about NSG and access control lists, see [https://azure.microsoft.com/enin/documentation/articles/virtual-networks-nsg/](https://azure.microsoft.com/enin/documentation/articles/virtual-networks-nsg/).   

## Traffic Flow Through Network Address Translation 
You can also request a Public IP (PIP) address for your NetScaler virtual machine (instance level). If you use this direct PIP at the VM level, you need not define inbound and outbound rules to intercept the network traffic. The incoming request from the Internet is received on the VM directly. Azure performs network address translation (NAT) and forwards the traffic to the internal IP address of the NetScaler instance. 