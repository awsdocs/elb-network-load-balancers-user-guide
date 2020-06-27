# What is a Network Load Balancer?<a name="introduction"></a>

Elastic Load Balancing supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. This guide discusses Network Load Balancers\. For more information about the other load balancers, see the [User Guide for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/) and the [User Guide for Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)\.

## Network Load Balancer components<a name="network-load-balancer-components"></a>

A *load balancer* serves as the single point of contact for clients\. The load balancer distributes incoming traffic across multiple targets, such as Amazon EC2 instances\. This increases the availability of your application\. You add one or more listeners to your load balancer\.

A *listener* checks for connection requests from clients, using the protocol and port that you configure, and forwards requests to a target group\.

Each *target group* routes requests to one or more registered targets, such as EC2 instances, using the TCP protocol and the port number that you specify\. You can register a target with multiple target groups\. You can configure health checks on a per target group basis\. Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer\.

For more information, see the following documentation:
+ [Load Balancers](network-load-balancers.md)
+ [Listeners](load-balancer-listeners.md)
+ [Target Groups](load-balancer-target-groups.md)

## Network Load Balancer overview<a name="network-load-balancer-overview"></a>

A Network Load Balancer functions at the fourth layer of the Open Systems Interconnection \(OSI\) model\. It can handle millions of requests per second\. After the load balancer receives a connection request, it selects a target from the target group for the default rule\. It attempts to open a TCP connection to the selected target on the port specified in the listener configuration\.

When you enable an Availability Zone for the load balancer, Elastic Load Balancing creates a load balancer node in the Availability Zone\. By default, each load balancer node distributes traffic across the registered targets in its Availability Zone only\. If you enable cross\-zone load balancing, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones\. For more information, see [Availability Zones](network-load-balancers.md#availability-zones)\.

If you enable multiple Availability Zones for your load balancer and ensure that each target group has at least one target in each enabled Availability Zone, this increases the fault tolerance of your applications\. For example, if one or more target groups does not have a healthy target in an Availability Zone, we remove the IP address for the corresponding subnet from DNS, but the load balancer nodes in the other Availability Zones are still available to route traffic\. If a client doesn't honor the time\-to\-live \(TTL\) and sends requests to the IP address after it is removed from DNS, the requests fail\.

For TCP traffic, the load balancer selects a target using a flow hash algorithm based on the protocol, source IP address, source port, destination IP address, destination port, and TCP sequence number\. The TCP connections from a client have different source ports and sequence numbers, and can be routed to different targets\. Each individual TCP connection is routed to a single target for the life of the connection\.

For UDP traffic, the load balancer selects a target using a flow hash algorithm based on the protocol, source IP address, source port, destination IP address, and destination port\. A UDP flow has the same source and destination, so it is consistently routed to a single target throughout its lifetime\. Different UDP flows have different source IP addresses and ports, so they can be routed to different targets\.

Elastic Load Balancing creates a network interface for each Availability Zone you enable\. Each load balancer node in the Availability Zone uses this network interface to get a static IP address\. When you create an Internet\-facing load balancer, you can optionally associate one Elastic IP address per subnet\.

When you create a target group, you specify its target type, which determines whether you register targets by instance ID or IP address\. If you register targets by instance ID, the source IP addresses of the clients are preserved and provided to your applications\. If you register targets by IP address, the source IP addresses are the private IP addresses of the load balancer nodes\.

You can add and remove targets from your load balancer as your needs change, without disrupting the overall flow of requests to your application\. Elastic Load Balancing scales your load balancer as traffic to your application changes over time\. Elastic Load Balancing can scale to the vast majority of workloads automatically\.

You can configure health checks, which are used to monitor the health of the registered targets so that the load balancer can send requests only to the healthy targets\.

For more information, see [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html) in the *Elastic Load Balancing User Guide*\.

## Benefits of migrating from a Classic Load Balancer<a name="network-load-balancer-benefits"></a>

Using a Network Load Balancer instead of a Classic Load Balancer has the following benefits:
+ Ability to handle volatile workloads and scale to millions of requests per second\.
+ Support for static IP addresses for the load balancer\. You can also assign one Elastic IP address per subnet enabled for the load balancer\.
+ Support for registering targets by IP address, including targets outside the VPC for the load balancer\.
+ Support for routing requests to multiple applications on a single EC2 instance\. You can register each instance or IP address with the same target group using multiple ports\.
+ Support for containerized applications\. Amazon Elastic Container Service \(Amazon ECS\) can select an unused port when scheduling a task and register the task with a target group using this port\. This enables you to make efficient use of your clusters\.
+ Support for monitoring the health of each service independently, as health checks are defined at the target group level and many Amazon CloudWatch metrics are reported at the target group level\. Attaching a target group to an Auto Scaling group enables you to scale each service dynamically based on demand\.

For more information about the features supported by each load balancer type, see [Product comparison](https://aws.amazon.com/elasticloadbalancing/details/#compare) for Elastic Load Balancing\.

## How to get started<a name="network-load-balancer-get-started"></a>

To create a Network Load Balancer, try one of the following tutorials:
+ [Getting started with Network Load Balancers](network-load-balancer-getting-started.md)
+ [Tutorial: Create a Network Load Balancer using the AWS CLI](network-load-balancer-cli.md)

For demos of common load balancer configurations, see [Elastic Load Balancing Demos](https://exampleloadbalancer.com/)\.

## Pricing<a name="network-load-balancer-pricing"></a>

For more information, see [Network Load Balancer Pricing](https://aws.amazon.com/elasticloadbalancing/networkloadbalancer/pricing/)\.