# Network Load Balancers<a name="network-load-balancers"></a>

A *load balancer* serves as the single point of contact for clients\. Clients send requests to the load balancer, and the load balancer sends them to targets, such as EC2 instances, in one or more Availability Zones\.

To configure your load balancer, you create [target groups](load-balancer-target-groups.md), and then register targets with your target groups\. Your load balancer is most effective if you ensure that each enabled Availability Zone has at least one registered target\. You also create [listeners](load-balancer-listeners.md) to check for connection requests from clients and route requests from clients to the targets in your target groups\.

Network Load Balancers support connections from clients over VPC peering, AWS managed VPN, and third\-party VPN solutions\. If the peered VPC is in the same region as the load balancer, the clients must be [Nitro instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#ec2-nitro-instances)\.

**Topics**
+ [Load Balancer State](#load-balancer-state)
+ [Load Balancer Attributes](#load-balancer-attributes)
+ [Availability Zones](#availability-zones)
+ [Deletion Protection](#deletion-protection)
+ [Connection Idle Timeout](#connection-idle-timeout)
+ [Create a Network Load Balancer](create-network-load-balancer.md)
+ [Tags for Your Network Load Balancer](load-balancer-tags.md)
+ [Delete a Network Load Balancer](load-balancer-delete.md)

## Load Balancer State<a name="load-balancer-state"></a>

A load balancer can be in one of the following states:

`provisioning`  
The load balancer is being set up\.

`active`  
The load balancer is fully set up and ready to route traffic\.

`failed`  
The load balancer could not be set up\.

## Load Balancer Attributes<a name="load-balancer-attributes"></a>

The following are the load balancer attributes:

`deletion_protection.enabled`  
Indicates whether deletion protection is enabled\. The default is `false`\.

`load_balancing.cross_zone.enabled`  
Indicates whether cross\-zone load balancing is enabled\. The default is `false`\.

## Availability Zones<a name="availability-zones"></a>

You enable one or more Availability Zones for your load balancer when you create it\. You cannot enable or disable Availability Zones for a Network Load Balancer after you create it\. If you enable multiple Availability Zones for your load balancer, this increases the fault tolerance of your applications\.

When you enable an Availability Zone, you specify one subnet from that Availability Zone\. Elastic Load Balancing creates a load balancer node in the Availability Zone and a network interface for the subnet \(the description starts with "ELB net" and includes the name of the load balancer\)\. Each load balancer node in the Availability Zone uses this network interface to get a static IP address\. Note that you can view this network interface but you cannot modify it\.

When you create an Internet\-facing load balancer, you can optionally associate one Elastic IP address per subnet\. You cannot add or change Elastic IP addresses for your subnets after you create the load balancer\.

**Requirements**
+ The subnets that you specify must have at least 8 available IP addresses\.
+ You can't specify subnets that were shared with you by another AWS account\.
+ You can't specify a subnet in a constrained Availability Zone\. The error message is "Load balancers with type 'network' are not supported in *az\_name*"\. You can specify a subnet in another Availability Zone that is not constrained and use cross\-zone load balancing to distribute traffic to targets in the constrained Availability Zone\.

### Cross\-Zone Load Balancing<a name="cross-zone-load-balancing"></a>

By default, each load balancer node distributes traffic across the registered targets in its Availability Zone only\. If you enable cross\-zone load balancing, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones\. For more information, see [Cross\-Zone Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#cross-zone-load-balancing) in the *Elastic Load Balancing User Guide*\.

**To enable cross\-zone load balancing using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Description**, **Edit attributes**\.

1. On the **Edit load balancer attributes** page, select **Enable** for **Cross\-Zone Load Balancing**, and choose **Save**\.

**To enable cross\-zone load balancing using the AWS CLI**  
Use the [modify\-load\-balancer\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-load-balancer-attributes.html) command with the `load_balancing.cross_zone.enabled` attribute\.

## Deletion Protection<a name="deletion-protection"></a>

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

## Connection Idle Timeout<a name="connection-idle-timeout"></a>

For each request that a client makes through a Network Load Balancer, the state of that connection is tracked\. The connection is terminated by the target\. If no data is sent through the connection by either the client or target for longer than the idle timeout, the connection is closed\. If a client sends data after the idle timeout period elapses, it receives a TCP RST packet to indicate that the connection is no longer valid\.

Elastic Load Balancing sets the idle timeout value to 350 seconds\. You cannot modify this value\. Your targets can use TCP keepalive packets to reset the idle timeout\.