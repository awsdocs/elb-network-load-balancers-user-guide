# Create a Network Load Balancer<a name="create-network-load-balancer"></a>

A load balancer takes requests from clients and distributes them across targets in a target group, such as EC2 instances\.

Before you begin, ensure that the virtual private cloud \(VPC\) for your load balancer has at least one public subnet in each Availability Zone where you have targets\.

To create a load balancer using the AWS CLI, see [Tutorial: Create a Network Load Balancer using the AWS CLI](network-load-balancer-cli.md)\.

To create a load balancer using the AWS Management Console, complete the following tasks\.

**Topics**
+ [Step 1: Configure a load balancer and a listener](#configure-load-balancer)
+ [Step 2: Configure a target group](#configure-target-group)
+ [Step 3: Register targets with the target group](#select-targets)
+ [Step 4: Create the load balancer](#create-load-balancer)

## Step 1: Configure a load balancer and a listener<a name="configure-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener configuration](load-balancer-listeners.md#listener-configuration)\.

**To configure your load balancer and listener**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. For **Network Load Balancer**, choose **Create**\.

1. For **Name**, type a name for your load balancer\. For example, **my\-nlb**\.

1. For **Scheme**, an internet\-facing load balancer routes requests from clients over the internet to targets\. An internal load balancer routes requests to targets using private IP addresses\.

1. For **Listeners**, the default is a listener that accepts TCP traffic on port 80\. You can keep the default listener settings, modify the protocol, or modify the port\. Choose **Add** to add another listener\.

1. For **Availability Zones**, select the VPC that you used for your EC2 instances\. For each Availability Zone that you used to launch your EC2 instances, select an Availability Zone and then select the public subnet for that Availability Zone\.

   By default, AWS assigns an IPv4 address to each load balancer node from the subnet for its Availability Zone\. Alternatively, if you create an internet\-facing load balancer, you can select an Elastic IP address for each Availability Zone\. This provides your load balancer with static IP addresses\. If you create an internal load balancer, you can assign a private IP address from the IPv4 range of each subnet instead of letting AWS assign one\.

1. Choose **Next: Configure Routing**\.

## Step 2: Configure a target group<a name="configure-target-group"></a>

You register targets, such as EC2 instances, with a target group\. The target group that you configure in this step is used as the target group in the listener rule, which forwards requests to the target group\. For more information, see [Target groups for your Network Load Balancers](load-balancer-target-groups.md)\.

**To configure your target group**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the target group\.

1. For **Protocol**, choose a protocol as follows:
   + If the listener protocol is TCP, choose **TCP** or **TCP\_UDP**\.
   + If the listener protocol is TLS, choose **TCP** or **TLS**\.
   + If the listener protocol is UDP, choose **UDP** or **TCP\_UDP**\.
   + If the listener protocol is TCP\_UDP, choose **TCP\_UDP**\.

1. \(Optional\) Set **Port** as needed\.

1. For **Target type**, select `instance` to specify targets by instance ID or `ip` to specify targets by IP address\. If the target group protocol is UDP or TCP\_UDP, you must select `instance`\.

1. For **Health checks**, keep the default health check settings\.

1. Choose **Next: Register Targets**\.

## Step 3: Register targets with the target group<a name="select-targets"></a>

You can register EC2 instances as targets in a target group\.

**To register targets by instance ID**

1. For **Instances**, select one or more instances\.

1. Keep the default instance listener port or type a new one and choose **Add to registered**\.

1. When you have finished registering instances, choose **Next: Review**\.

**To register targets by IP address**

1. For each IP address to register, do the following:

   1. For **Network**, if the IP address is from a subnet of the target group VPC, select the VPC\. Otherwise, select **Other private IP address**\.

   1. For **Availability Zone**, select an Availability Zone or **all**\. This determines whether the target receives traffic from the load balancer nodes in the specified Availability Zone only or from all enabled Availability Zones\. This field is not displayed if you are registering IP addresses from the VPC\. In this case, the Availability Zone is automatically detected\.

   1. For **IP**, type the address\.

   1. For **Port**, type the port\.

   1. Choose **Add to list**\.

1. When you have finished adding IP addresses to the list, choose **Next: Review**\.

## Step 4: Create the load balancer<a name="create-load-balancer"></a>

After creating your load balancer, you can verify that your EC2 instances have passed the initial health check and then test that the load balancer is sending traffic to your EC2 instances\. When you are finished with your load balancer, you can delete it\. For more information, see [Delete a Network Load Balancer](load-balancer-delete.md)\.

**To create the load balancer**

1. On the **Review** page, choose **Create**\.

1. After the load balancer is created, choose **Close**\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's probably because the instance is still in the process of being registered, or it has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is healthy, you can test your load balancer\.