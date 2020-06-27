# Network Load Balancers<a name="network-load-balancers"></a>

A *load balancer* serves as the single point of contact for clients\. Clients send requests to the load balancer, and the load balancer sends them to targets, such as EC2 instances, in one or more Availability Zones\.

To configure your load balancer, you create [target groups](load-balancer-target-groups.md), and then register targets with your target groups\. Your load balancer is most effective if you ensure that each enabled Availability Zone has at least one registered target\. You also create [listeners](load-balancer-listeners.md) to check for connection requests from clients and route requests from clients to the targets in your target groups\.

Network Load Balancers support connections from clients over VPC peering, AWS managed VPN, AWS Direct Connect, and third\-party VPN solutions\.

**Topics**
+ [Load balancer state](#load-balancer-state)
+ [Load balancer attributes](#load-balancer-attributes)
+ [Availability Zones](#availability-zones)
+ [Deletion protection](#deletion-protection)
+ [Connection idle timeout](#connection-idle-timeout)
+ [DNS name](#dns-name)
+ [Create a Network Load Balancer](create-network-load-balancer.md)
+ [Tags for your Network Load Balancer](load-balancer-tags.md)
+ [Delete a Network Load Balancer](load-balancer-delete.md)

## Load balancer state<a name="load-balancer-state"></a>

A load balancer can be in one of the following states:

`provisioning`  
The load balancer is being set up\.

`active`  
The load balancer is fully set up and ready to route traffic\.

`failed`  
The load balancer could not be set up\.

## Load balancer attributes<a name="load-balancer-attributes"></a>

The following are the load balancer attributes:

`deletion_protection.enabled`  
Indicates whether [deletion protection](#deletion-protection) is enabled\. The default is `false`\.

`load_balancing.cross_zone.enabled`  
Indicates whether [cross\-zone load balancing](#cross-zone-load-balancing) is enabled\. The default is `false`\.

## Availability Zones<a name="availability-zones"></a>

You enable one or more Availability Zones for your load balancer when you create it\. If you enable multiple Availability Zones for your load balancer, this increases the fault tolerance of your applications\. You cannot disable Availability Zones for a Network Load Balancer after you create it, but you can enable additional Availability Zones\.

When you enable an Availability Zone, you specify one subnet from that Availability Zone\. Elastic Load Balancing creates a load balancer node in the Availability Zone and a network interface for the subnet \(the description starts with "ELB net" and includes the name of the load balancer\)\. Each load balancer node in the Availability Zone uses this network interface to get an IPv4 address\. Note that you can view this network interface but you cannot modify it\.

When you create an internet\-facing load balancer, you can optionally specify one Elastic IP address per subnet\. If you do not choose one of your own Elastic IP addresses, Elastic Load Balancing provides one Elastic IP address per subnet for you\. These Elastic IP addresses provide your load balancer with static IP addresses that will not change during the life of the load balancer\. You cannot change these Elastic IP addresses after you create the load balancer\.

When you create an internal load balancer, you can optionally specify one private IP address per subnet\. If you do not specify an IP address from the subnet, Elastic Load Balancing chooses one for you\. These private IP addresses provide your load balancer with static IP addresses that will not change during the life of the load balancer\. You cannot change these private IP addresses after you create the load balancer\.

**Requirements**
+ For internet\-facing load balancers, the subnets that you specify must have at least 8 available IP addresses\. For internal load balancers, this is only required if you let AWS select a private IPv4 address from the subnet\.
+ You can't specify a subnet in a constrained Availability Zone\. The error message is "Load balancers with type 'network' are not supported in *az\_name*"\. You can specify a subnet in another Availability Zone that is not constrained and use cross\-zone load balancing to distribute traffic to targets in the constrained Availability Zone\.
+ You can't specify a subnet in a Local Zone\.

After you enable an Availability Zone, the load balancer starts routing requests to the registered targets in that Availability Zone\. Your load balancer is most effective if you ensure that each enabled Availability Zone has at least one registered target\.

**To add Availability Zones using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. On the **Description** tab, under **Basic Configuration**, choose **Edit subnets**\.

1. To enable an Availability Zone, select the check box for that Availability Zone\. If there is one subnet for that Availability Zone, it is selected\. If there is more than one subnet for that Availability Zone, select one of the subnets\. Note that you can select only one subnet per Availability Zone\.

   For an internet\-facing load balancer, you can select an Elastic IP address for each Availability Zone\. For an internal load balancer, you can assign a private IP address from the IPv4 range of each subnet instead of letting Elastic Load Balancing assign one\.

1. Choose **Save**\.

**To add Availability Zones using the AWS CLI**  
Use the [set\-subnets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/set-subnets.html) command\.

### Cross\-zone load balancing<a name="cross-zone-load-balancing"></a>

By default, each load balancer node distributes traffic across the registered targets in its Availability Zone only\. If you enable cross\-zone load balancing, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones\. For more information, see [Cross\-zone load balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#cross-zone-load-balancing) in the *Elastic Load Balancing User Guide*\.

**To enable cross\-zone load balancing using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Description**, **Edit attributes**\.

1. On the **Edit load balancer attributes** page, select **Enable** for **Cross\-Zone Load Balancing**, and choose **Save**\.

**To enable cross\-zone load balancing using the AWS CLI**  
Use the [modify\-load\-balancer\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-load-balancer-attributes.html) command with the `load_balancing.cross_zone.enabled` attribute\.

## Deletion protection<a name="deletion-protection"></a>

To prevent your load balancer from being deleted accidentally, you can enable deletion protection\. By default, deletion protection is disabled for your load balancer\.

If you enable deletion protection for your load balancer, you must disable it before you can delete the load balancer\.

**To enable deletion protection using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Description**, **Edit attributes**\.

1. On the **Edit load balancer attributes** page, select **Enable** for **Delete Protection**, and choose **Save**\.

**To disable deletion protection using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Description**, **Edit attributes**\.

1. On the **Edit load balancer attributes** page, clear **Enable delete protection** and choose **Save**\.

**To enable or disable deletion protection using the AWS CLI**  
Use the [modify\-load\-balancer\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-load-balancer-attributes.html) command with the `deletion_protection.enabled` attribute\.

## Connection idle timeout<a name="connection-idle-timeout"></a>

For each TCP request that a client makes through a Network Load Balancer, the state of that connection is tracked\. If no data is sent through the connection by either the client or target for longer than the idle timeout, the connection is closed\. If a client or a target sends data after the idle timeout period elapses, it receives a TCP RST packet to indicate that the connection is no longer valid\.

Elastic Load Balancing sets the idle timeout value for TCP flows to 350 seconds\. You cannot modify this value\. Clients or targets can use TCP keepalive packets to reset the idle timeout\.

While UDP is connectionless, the load balancer maintains UDP flow state based on the source and destination IP addresses and ports, ensuring that packets that belong to the same flow are consistently sent to the same target\. After the idle timeout period elapses, the load balancer considers the incoming UDP packet as a new flow and routes it to a new target\. Elastic Load Balancing sets the idle timeout value for UDP flows to 120 seconds\.

EC2 instances must respond to a new request within 30 seconds in order to establish a return path\.

## DNS name<a name="dns-name"></a>

Each Network Load Balancer receives a default Domain Name System \(DNS\) name with the following syntax: *name*\-*id*\.elb\.*region*\.amazonaws\.com\. For example, my\-load\-balancer\-1234567890abcdef\.elb\.us\-east\-2\.amazonaws\.com\.

If you'd prefer to use a DNS name that is easier to remember, you can create a custom domain name and associate it with the DNS name for your load balancer\. When a client makes a request using this custom domain name, the DNS server resolves it to the DNS name for your load balancer\.

First, register a domain name with an accredited domain name registrar\. Next, use your DNS service, such as your domain registrar, to create a CNAME record to route requests to your load balancer\. For more information, see the documentation for your DNS service\. For example, you can use Amazon Route 53 as your DNS service\. For more information, see [Routing traffic to an ELB load balancer](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-elb-load-balancer.html) in the *Amazon Route 53 Developer Guide*\.

The load balancer has one IP address per enabled Availability Zone\. These are the addresses of the load balancer nodes\. The DNS name of the load balancer resolves to these addresses\. For example, suppose that the custom domain name for your load balancer is `example.networkloadbalancer.com`\. Use the following dig or nslookup command to determine the IP addresses of the load balancer nodes\.

**Linux or Mac**

```
$ dig +short example.networkloadbalancer.com
```

**Windows**

```
C:\> nslookup example.networkloadbalancer.com
```

The load balancer has DNS records for its load balancer nodes\. You can use DNS names with the following syntax to determine the IP addresses of the load balancer nodes: *az*\.*name*\-*id*\.elb\.*region*\.amazonaws\.com\.

**Linux or Mac**

```
$ dig +short us-east-2b.my-load-balancer-1234567890abcdef.elb.us-east-2.amazonaws.com
```

**Windows**

```
C:\> nslookup us-east-2b.my-load-balancer-1234567890abcdef.elb.us-east-2.amazonaws.com
```