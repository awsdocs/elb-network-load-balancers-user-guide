# Getting started with Network Load Balancers<a name="network-load-balancer-getting-started"></a>

This tutorial provides a hands\-on introduction to Network Load Balancers through the AWS Management Console, a web\-based interface\. To create your first Network Load Balancer, complete the following steps\.

**Topics**
+ [Before you begin](#prerequisites)
+ [Step 1: Choose a load balancer type](#select-load-balancer-type)
+ [Step 2: Configure your load balancer and listener](#configure-load-balancer)
+ [Step 3: Configure your target group](#configure-target-group)
+ [Step 4: Register targets with your target group](#add-targets)
+ [Step 5: Create and test your load balancer](#test-load-balancer)
+ [Step 6: Delete your load balancer \(optional\)](#delete-load-balancer)

Alternatively, to create an Application Load Balancer, see [Getting started with Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancer-getting-started.html) in the *User Guide for Application Load Balancers*\. To create a Classic Load Balancer, see [Create a Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html) in the *User Guide for Classic Load Balancers*\.

For demos of common load balancer configurations, see [Elastic Load Balancing Demos](https://exampleloadbalancer.com/)\.

## Before you begin<a name="prerequisites"></a>
+ Decide which Availability Zones you will use for your EC2 instances\. Configure your virtual private cloud \(VPC\) with at least one public subnet in each of these Availability Zones\. These public subnets are used to configure the load balancer\. You can launch your EC2 instances in other subnets of these Availability Zones instead\.
+ Launch at least one EC2 instance in each Availability Zone\. Ensure that the security groups for these instances allow TCP access from clients on the listener port and health check requests from your VPC\. For more information, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

## Step 1: Choose a load balancer type<a name="select-load-balancer-type"></a>

Elastic Load Balancing supports three types of load balancers\. For this tutorial, you create a Network Load Balancer\.

**To create a Network Load Balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar, choose a region for your load balancer\. Be sure to choose the same region that you used for your EC2 instances\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. For **Network Load Balancer**, choose **Create**\.

## Step 2: Configure your load balancer and listener<a name="configure-load-balancer"></a>

On the **Configure Load Balancer** page, complete the following procedure\.

**To configure your load balancer and listener**

1. For **Name**, type a name for your load balancer\.

   The name of your Network Load Balancer must be unique within your set of Application Load Balancers and Network Load Balancers for the region, can have a maximum of 32 characters, can contain only alphanumeric characters and hyphens, must not begin or end with a hyphen, and must not begin with "internal\-"\.

1. For **Scheme**, keep the default value, **internet\-facing**\.

1. For **Listeners**, keep the default, which is a listener that accepts TCP traffic on port 80\.

1. For **Availability Zones**, select the VPC that you used for your EC2 instances\. For each Availability Zone that you used to launch your EC2 instances, select the Availability Zone and then select one public subnet for that Availability Zone\.

   By default, AWS assigns an IPv4 address to each load balancer node from the subnet for its Availability Zone\. Alternatively, when you create an internet\-facing load balancer, you can select an Elastic IP address for each Availability Zone\. This provides your load balancer with static IP addresses\.

1. Choose **Next: Configure Routing**\.

## Step 3: Configure your target group<a name="configure-target-group"></a>

Create a target group, which is used in request routing\. The rule for your listener routes requests to the registered targets in this target group\. The load balancer checks the health of targets in this target group using the health check settings defined for the target group\. On the **Configure Routing** page, complete the following procedure\.

**To configure your target group**

1. For **Target group**, keep the default, **New target group**\.

1. For **Name**, type a name for the new target group\.

1. Keep **Protocol** as TCP, **Port** as 80, and **Target type** as instance\.

1. For **Health checks**, keep the default protocol\.

1. Choose **Next: Register Targets**\.

## Step 4: Register targets with your target group<a name="add-targets"></a>

On the **Register Targets** page, complete the following procedure\.

**To register targets with the target group**

1. For **Instances**, select one or more instances\.

1. Keep the default port, 80, and choose **Add to registered**\.

1. When you have finished selecting instances, choose **Next: Review**\.

## Step 5: Create and test your load balancer<a name="test-load-balancer"></a>

Before creating the load balancer, review your settings\. After creating the load balancer, verify that it's sending traffic to your EC2 instances\.

**To create and test your load balancer**

1. On the **Review** page, choose **Create**\.

1. After you are notified that your load balancer was created successfully, choose **Close**\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's probably because the instance is still in the process of being registered, or it has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is `healthy`, you can test your load balancer\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the newly created load balancer\.

1. Choose **Description** and copy the DNS name of the load balancer \(for example, my\-load\-balancer\-1234567890abcdef\.elb\.us\-east\-2\.amazonaws\.com\)\. Paste the DNS name into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your server\.

## Step 6: Delete your load balancer \(optional\)<a name="delete-load-balancer"></a>

As soon as your load balancer becomes available, you are billed for each hour or partial hour that you keep it running\. When you no longer need a load balancer, you can delete it\. As soon as the load balancer is deleted, you stop incurring charges for it\. Note that deleting a load balancer does not affect the targets registered with the load balancer\. For example, your EC2 instances continue to run\.

**To delete your load balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.