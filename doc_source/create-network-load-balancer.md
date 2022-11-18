# Create a Network Load Balancer<a name="create-network-load-balancer"></a>

A load balancer takes requests from clients and distributes them across targets in a target group, such as EC2 instances\.

Before you begin, ensure that the virtual private cloud \(VPC\) for your load balancer has at least one public subnet in each Availability Zone where you have targets\. You must also configure a target group and register at least one target to set as default in order to route your traffic to the target group\.

To create a load balancer using the AWS CLI, see [Tutorial: Create a Network Load Balancer using the AWS CLI](network-load-balancer-cli.md)\.

To create a load balancer using the AWS Management Console, complete the following tasks\.

**Topics**
+ [Step 1: Configure a target group](#configure-target-group)
+ [Step 2: Register targets](#select-targets)
+ [Step 3: Configure a load balancer and a listener](#configure-load-balancer)
+ [Step 4: Test the load balancer](#test-load-balancer)

## Step 1: Configure a target group<a name="configure-target-group"></a>

Configuring a target group allows you to register targets such as EC2 instances\. The target group that you configure in this step is used as the target group in the listener rule when you configure your load balancer\. For more information, see [Target groups for your Network Load Balancers](load-balancer-target-groups.md)\.

**To configure your target group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. On the **Basic configuration** pane, do the following:

   1. For **Choose a target type**, select **Instances** to register targets by instance ID, **IP addresses** to register targets by IP address, or **Application Load Balancer** to register an Application Load Balancer as a target\.

   1. For **Target group name**, enter a name for the target group\.

   1. For **Protocol**, choose a protocol as follows:
      + If the listener protocol is TCP, choose **TCP** or **TCP\_UDP**\.
      + If the listener protocol is TLS, choose **TCP** or **TLS**\.
      + If the listener protocol is UDP, choose **UDP** or **TCP\_UDP**\.
      + If the listener protocol is TCP\_UDP, choose **TCP\_UDP**\.

   1. \(Optional\) For **Port**, modify the default value as needed\.

   1. For **VPC**, select a virtual private cloud \(VPC\) with the targets that you want to register with your target group\.

   1. For **Protocol version**, select **HTTP1** when the request protocol is HTTP/1\.1 or HTTP/2; select **HTTP2**, when the request protocol is HTTP/2 or gRPC; and select **gRPC**, when the request protocol is gRPC\. 

1. In the **Health checks** section, modify the default settings as needed\. For **Advanced health check settings**, choose the health check port, count, timeout, interval, and specify success codes\. If health checks consecutively exceed the **Unhealthy threshold** count, the load balancer takes the target out of service\. If health checks consecutively exceed the **Healthy threshold** count, the load balancer puts the target back in service\. For more information, see [Health checks for your target groups](target-group-health-checks.md)\.

1. \(Optional\) Add one or more tags as follows:

   1. Expand the **Tags** section\.

   1. Choose **Add tag**\.

   1. Enter the tag **Key** and tag **Value**\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

1. Choose **Next**\.

## Step 2: Register targets<a name="select-targets"></a>

You can register EC2 instances, IP addresses, or an Application Load Balancer with your target group\. This is an optional step to create a load balancer\. However, you must register your targets to ensure that your load balancer can route traffic to them\.

1. In the **Register targets** page, add one or more targets as follows:
   + If the target type is **Instances**, select one or more instances, enter one or more ports, and then choose **Include as pending below**\.
   + If the target type is **IP addresses**, select the network, enter the IP address and ports, and then choose **Include as pending below**\.
   + If the target type is **Application Load Balancer**, select an Application Load Balancer\.

1. Choose **Create target group**\.

## Step 3: Configure a load balancer and a listener<a name="configure-load-balancer"></a>

To create a Network Load Balancer, you must first provide basic configuration information for your load balancer, such as a name, scheme, and IP address type\. Then provide information about your network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener configuration](load-balancer-listeners.md#listener-configuration)\.

****To configure your load balancer and listener****

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. Under **Network Load Balancer**, choose **Create**\.

1. **Basic configuration**

   1. For **Load balancer name**, enter a name for your load balancer\. For example, **my\-nlb**\. The name of your Network Load Balancer must be unique within your set of Application Load Balancers and Network Load Balancers for the Region\. It can have a maximum of 32 characters, and contain only alphanumeric characters and hyphens\. It must not begin or end with a hyphen, or with `internal-`\.

   1. For **Scheme**, choose **Internet\-facing** or **Internal**\. An internet\-facing load balancer routes requests from clients to targets over the internet\. An internal load balancer routes requests to targets using private IP addresses\.

   1. For **IP address type**, choose **IPv4** or **Dualstack**\. Use **IPv4** if your clients use IPv4 addresses to communicate with the load balancer\. Use **Dualstack** if your clients use both IPv4 and IPv6 addresses to communicate with the load balancer\. 

1. **Network and security**

   1. For **VPC**, select the VPC that you used for your EC2 instances\. If you selected **Internet\-facing** for **Scheme**, only VPCs with an internet gateway are available for selection\.

   1. For **Mappings**, select one or more Availability Zones and corresponding subnets\. Enabling multiple Availability Zones increases the fault tolerance of your applications\. For internet\-facing load balancers, you can select an Elastic IP address for each Availability Zone\. This provides your load balancer with static IP addresses\. Alternatively, for an internal load balancer, you can assign a private IP address from the IPv4 range of each subnet instead of letting AWS assign one for you\.

1. **Listeners and routing**

   1. For **Listeners**, the default is a listener that accepts TCP traffic on port 80\. You can keep the default listener settings, modify the protocol, or modify the port\. 

   1. For **Default action**, select a target group to forward traffic\. If you didn't create a target group previously, you must create one now\. You can optionally choose **Add listener** to add another listener \(for example, a TLS listener\)\.

   1. For **Secure listener settings** \(available only for TLS listeners\), choose a **Security policy** that meets your requirements\.

   1. For **ALPN policy**, choose a policy to enable ALPN or choose **None** to disable ALPN\.

   1. For **Default SSL certificate**, choose **From ACM** \(recommended\) and select a certificate\. If you don't have a certificate that is available to choose, you can import a certificate into ACM, or use ACM to provision one for you\. For more information, see [Issuing and Managing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *ACM User Guide*\.

1. **Tag and create**

   1. \(Optional\) Add a tag to categorize your load balancer\. Tag keys must be unique for each load balancer\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

   1. Review your configuration, and choose **Create load balancer**\. A few default attributes are applied to your load balancer during creation\. You can view and edit them after creating the load balancer\. For more information, see [Load balancer attributes](network-load-balancers.md#load-balancer-attributes)\.

## Step 4: Test the load balancer<a name="test-load-balancer"></a>

After creating your load balancer, you can verify that your EC2 instances have passed the initial health check and then test that the load balancer is sending traffic to your EC2 instances\. To delete the load balancer, see [Delete a Network Load Balancer](load-balancer-delete.md)\.

**To test the load balancer**

1. After the load balancer is created, choose **Close**\.

1. In the left navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's probably because the instance is still in the process of being registered, or it has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is healthy, you can test your load balancer\. For more information, see [Target health status](target-group-health-checks.md#target-health-states)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Select the newly created load balancer\.

1. Copy the DNS name of the load balancer \(for example, my\-load\-balancer\-1234567890abcdef\.elb\.us\-east\-2\.amazonaws\.com\)\. Paste the DNS name into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your server\.