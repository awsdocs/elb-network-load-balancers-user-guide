# Getting started with Network Load Balancers<a name="network-load-balancer-getting-started"></a>

This tutorial provides a hands\-on introduction to Network Load Balancers through the AWS Management Console, a web\-based interface\. To create your first Network Load Balancer, complete the following steps\.

**Topics**
+ [Before you begin](#prerequisites)
+ [Step 1: Configure your target group](#configure-target-group)
+ [Step 2: Choose a load balancer type](#select-load-balancer-type)
+ [Step 3: Configure your load balancer and listener](#configure-load-balancer-listener)
+ [Step 4: Test your load balancer](#test-load-balancer)
+ [Step 5: \(Optional\) Delete your load balancer](#delete-load-balancer)

For demos of common load balancer configurations, see [Elastic Load Balancing Demos](https://exampleloadbalancer.com/)\.

## Before you begin<a name="prerequisites"></a>
+ Decide which Availability Zones you will use for your EC2 instances\. Configure your virtual private cloud \(VPC\) with at least one public subnet in each of these Availability Zones\. These public subnets are used to configure the load balancer\. You can launch your EC2 instances in other subnets of these Availability Zones instead\.
+ Launch at least one EC2 instance in each Availability Zone\. Ensure that the security groups for these instances allow TCP access from clients on the listener port and health check requests from your VPC\. For more information, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

## Step 1: Configure your target group<a name="configure-target-group"></a>

Create a target group, which is used in request routing\. The rule for your listener routes requests to the registered targets in this target group\. The load balancer checks the health of targets in this target group using the health check settings defined for the target group\. 

**To configure your target group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. Keep the target type as **instances**\.

1. For **Target group name**, enter a name for the new target group\.

1. For **Protocol**, choose **TCP**, and for **Port**, choose **80**\.

1. For **VPC** select the VPC that contains your instances\. Keep **Protocol version** as **HTTP1**\.

1. For **Health checks**, keep the default settings\.

1. Choose **Next**\.

1. On the **Register targets** page, complete the following steps\. This is an optional step to create a target group\. However, you must register your targets if you want to test your load balancer and ensure that it is routing traffic to your targets\.

   1. For **Available instances**, select one or more instances\.

   1. Keep the default port 80, and choose **Include as pending below**\.

1. Choose **Create target group**\.

## Step 2: Choose a load balancer type<a name="select-load-balancer-type"></a>

Elastic Load Balancing supports different types of load balancers\. For this tutorial, you create a Network Load Balancer\.

**To create a Network Load Balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar, choose a Region for your load balancer\. Be sure to choose the same Region that you used for your EC2 instances\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create load balancer**\.

1. For **Network Load Balancer**, choose **Create**\.

## Step 3: Configure your load balancer and listener<a name="configure-load-balancer-listener"></a>

To create a Network Load Balancer, you must first provide basic configuration information for your load balancer, such as a name, scheme, and IP address type\. Then provide information about your network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener configuration](load-balancer-listeners.md#listener-configuration)\.

**To configure your load balancer and listener**

1. For **Load balancer name**, enter a name for your load balancer\. For example, `my-nlb`\. 

1. For **Scheme** and **IP address type**, keep the default values\.

1. For **Network mapping**, select the VPC that you used for your EC2 instances\. For each Availability Zone that you used to launch your EC2 instances, select the Availability Zone and then select one public subnet for that Availability Zone\.

   By default, AWS assigns an IPv4 address to each load balancer node from the subnet for its Availability Zone\. Alternatively, when you create an internet\-facing load balancer, you can select an Elastic IP address for each Availability Zone\. This provides your load balancer with static IP addresses\.

1. For **Listeners and routing**, keep the default protocol and port, and select your target group from the list\. This configures a listener that accepts TCP traffic on port 80 and forwards traffic to the selected target group by default\.

1. For **Default action**, select the target group that you created and registered in step 1\. 

1. \(Optional\) Add tags to categorize your load balancer\. Tag keys must be unique for each load balancer\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

1. Review your configuration, and choose **Create load balancer**\. A few default attributes are applied to your load balancer during creation\. You can view and edit them after creating the load balancer\. For more information, see [Load balancer attributes](network-load-balancers.md#load-balancer-attributes)\.

## Step 4: Test your load balancer<a name="test-load-balancer"></a>

After creating the load balancer, verify that it's sending traffic to your EC2 instances\.

**To test your load balancer**

1. After you are notified that your load balancer was created successfully, choose **Close**\.

1. In the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's probably because the instance is still in the process of being registered, or it has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is `healthy`, you can test your load balancer\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Select the name of the newly created load balancer to open its details page\.

1. Copy the DNS name of the load balancer \(for example, my\-load\-balancer\-1234567890abcdef\.elb\.us\-east\-2\.amazonaws\.com\)\. Paste the DNS name into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your server\.

## Step 5: \(Optional\) Delete your load balancer<a name="delete-load-balancer"></a>

As soon as your load balancer becomes available, you are billed for each hour or partial hour that you keep it running\. When you no longer need a load balancer, you can delete it\. As soon as the load balancer is deleted, you stop incurring charges for it\. Note that deleting a load balancer does not affect the targets registered with the load balancer\. For example, your EC2 instances continue to run\.

**To delete your load balancer**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Select the checkbox for the load balancer, and choose **Actions**, **Delete**\.

1. When prompted for confirmation, enter **confirm** and choose **Delete**\.