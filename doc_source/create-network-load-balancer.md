# Create a Network Load Balancer<a name="create-network-load-balancer"></a>

A load balancer takes requests from clients and distributes them across targets in a target group, such as EC2 instances\.

Before you begin, launch EC2 instances in at least one Availability Zone\. Ensure that the virtual private cloud \(VPC\) has at least one public subnet in each of these Availability Zones\.

To create a load balancer using the AWS CLI, see [Tutorial: Create a Network Load Balancer Using the AWS CLI](network-load-balancer-cli.md)\.

To create a load balancer using the AWS Management Console, complete the following tasks\.


+ [Step 1: Configure a Load Balancer and a Listener](#configure-load-balancer)
+ [Step 2: Configure a Target Group](#configure-target-group)
+ [Step 3: Register Targets with the Target Group](#select-targets)
+ [Step 4: Create the Load Balancer](#create-load-balancer)

## Step 1: Configure a Load Balancer and a Listener<a name="configure-load-balancer"></a>

First, provide some basic configuration information for your load balancer, such as a name, a network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener Configuration](load-balancer-listeners.md#listener-configuration)\.

**To configure your load balancer and listener**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. For **Network Load Balancer**, choose **Create**\.

1. For **Name**, type a name for your load balancer\. For example, **my\-nlb**\.

1. For **Scheme**, an internet\-facing load balancer routes requests from clients over the internet to targets\. An internal load balancer routes requests to targets using private IP addresses\.

1. For **Listeners**, the default is a listener that accepts TCP traffic on port 80\. You can keep the default listener settings, modify the protocol, or modify the port\. Choose **Add** to add another listener\.

1. For **Availability Zones**, select the VPC that you used for your EC2 instances\. For each Availability Zone that you used to launch your EC2 instances, select an Availability Zone and then select the public subnet for that Availability Zone\. To associate an Elastic IP address with the subnet, select it from **Elastic IP**\.

1. Choose **Next: Configure Routing**\.

## Step 2: Configure a Target Group<a name="configure-target-group"></a>

You register targets, such as EC2 instances, with a target group\. The target group that you configure in this step is used as the target group in the listener rule, which forwards requests to the target group\. For more information, see [Target Groups for Your Network Load Balancers](load-balancer-target-groups.md)\.

**To configure your target group**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the target group\.

1. Set **Protocol** and **Port** as needed\.

1. For **Target type**, select `instance` to specify targets by instance ID or `ip` to specify targets by IP address\.

1. For **Health checks**, keep the default health check settings\.

1. Choose **Next: Register Targets**\.

## Step 3: Register Targets with the Target Group<a name="select-targets"></a>

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

## Step 4: Create the Load Balancer<a name="create-load-balancer"></a>

After creating your load balancer, you can verify that your EC2 instances have passed the initial health check and then test that the load balancer is sending traffic to your EC2 instances\. When you are finished with your load balancer, you can delete it\. For more information, see [Delete a Network Load Balancer](load-balancer-delete.md)\.

**To create the load balancer**

1. On the **Review** page, choose **Create**\.

1. After the load balancer is created, choose **Close**\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's probably because the instance is still in the process of being registered, or it has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is healthy, you can test your load balancer\.