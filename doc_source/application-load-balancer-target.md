# Application Load Balancers as targets<a name="application-load-balancer-target"></a>

You can create a target group with a single Application Load Balancer as the target, and configure your Network Load Balancer to forward traffic to it\. In this scenario, the Application Load Balancer takes over the load balancing decision as soon as traffic reaches it\. This configuration combines the features of both load balancers and offers the following advantages:
+ You can use the layer 7 request\-based routing feature of the Application Load Balancer in combination with features that the Network Load Balancer supports, such as endpoint services \(AWS PrivateLink\) and static IP addresses\.
+ The configuration works well for applications that use multi\-protocol connections, such as media services using HTTP for signaling, and RTP to stream content\.

You can use this feature with an internal or internet\-facing Application Load Balancer as the target of an internal or internet\-facing Network Load Balancer\.

## Step 1: Create the Application Load Balancer<a name="create-new-application-load-balancer-target"></a>

Before you begin, configure the target groups that this Application Load Balancer will route traffic to\. Ensure that you have a virtual private cloud \(VPC\) with the instances that you want to include in the target group, and at least one public subnet in each of the Availability Zones used by your targets\. 

**To create the Application Load Balancer using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. Under **Application Load Balancer**, choose **Create**\.

1. On the **Create Application Load Balancer** page, enter **Basic configuration** information such as **Name**, **Scheme**, and **IP address type**\. 

1. For **Listeners**, you can create an HTTP or HTTPS listener on any port\. However, you must ensure that the port number of this listener matches the port of the target group in which this Application Load Balancer will reside\.

1. Under **Availability Zones**, do the following:

   1. For **VPC**, select a virtual private cloud \(VPC\) with instances or IP addresses that you included as targets of your Application Load Balancer\. You must use the same VPC that you would use for your Network Load Balancer in [Step 3: Create a Network Load Balancer, and configure the Application Load Balancer as its target](#configure-application-load-balancer-target)\.

   1. Select two or more **Availability Zones** and corresponding subnets\. Ensure that these Availability Zones match those enabled for your Network Load Balancer to optimize availability, scaling, and performance\.

1. You can **Assign a security group** to your load balancer by creating a new security group or by selecting an existing one\. 

   The security group that you select should contain a rule that allows traffic to the listener port for this load balancer\. Use the CIDR blocks \(IP address range\) of the client's computers as the traffic source in the inbound rules for security groups\. This allows clients to send traffic through this Application Load Balancer\. For more information on configuring security groups for an Application Load Balancer as a target of a Network Load Balancer, see [Security groups for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-update-security-groups.html) in the *User Guide for Application Load Balancers*\.

1. For **Configure Routing**, select the target group that you configured for this Application Load Balancer\. If you don't have an available target group, and want to configure a new one, see [Create a target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-target-group.html) in the *User Guide for Application Load Balancers*\.

1. Review your configuration, and choose **Create load balancer**\.

**To create the Application Load Balancer using the AWS CLI**  
Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command\.

## Step 2: Create the target group with the Application Load Balancer as the target<a name="register-application-load-balancer-target"></a>

Creating a target group allows you to register a new or existing Application Load Balancer as a target\. You can only add one Application Load Balancer per target group\. The same Application Load Balancer can also be used in a separate target group, as the target of up to two Network Load Balancers\.

**To create a target group and register the Application Load Balancer as a target, using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   Note that for this feature, you must turn on the **New EC2 Experience**\. Use the toggle button on the top left corner of the navigation pane to switch to the new console\.

1. On the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. On the **Specify group details** page, under **Basic configuration**, choose **Application Load Balancer**\.

1. For **Target group name**, enter a name for the Application Load Balancer target group\.

1. For **Protocol**, only TCP is allowed\. Select the **Port** for your target group\. This target group port must match the listener port of the Application Load Balancer\. Alternatively, you can add or edit the listener port on the Application Load Balancer to match this port\.

1. For VPC, select the virtual private cloud \(VPC\) with the Application Load Balancer that you want to include in the target group\.

1. For **Health checks**, choose HTTP or HTTPS as the **Health check protocol**\. Health checks are sent to the Application Load Balancer and forwarded to its targets using the specified port, protocol, and ping path\. Ensure that your Application Load Balancer can receive these health checks by having a listener with a port and protocol that matches the health check port and protocol\.

   For **Advanced health check settings**, you cannot modify **Unhealthy threshold**, **Timeout**, or **Success codes**\. For more information, see [Health checks for your target groups](target-group-health-checks.md)\. 

1. \(Optional\) Add one or more tags as required\.

1. Choose **Next**\.

1. On the **Register targets** page, choose the Application Load Balancer that you want to register as the target\. The Application Load Balancer you choose from the list must have a listener on the same port as the target group you’re creating\. You can add or edit a listener on this load balancer to match the target group’s port or return to the previous step and change the port that's specified for the target group\. If you're unsure about which Application Load Balancer to add as the target, or don't want to add it at this point, you can choose to add the Application Load Balancer later\. 

1. Choose **Create target group**\.

**Important**  
If at any point you need to delete an Application Load Balancer, be aware that you cannot delete an Application Load Balancer that has been registered as a target in a target group\. To delete it, you must first deregister it from all target groups\. 

**To create a target group and register the Application Load Balancer as a target, using the AWS CLI**  
Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) and [register\-targets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command\.

## Step 3: Create a Network Load Balancer, and configure the Application Load Balancer as its target<a name="configure-application-load-balancer-target"></a>

Use the following steps to create the Network Load Balancer and then configure the Application Load Balancer as its target using the console\.

**To create your Network Load Balancer and listener using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   Note that for this feature, you must turn on the **New EC2 Experience**\. Use the toggle button on the top left corner of the navigation pane to switch to the new console\.

1. On the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. Under **Network Load Balancer**, choose **Create**\.

1. **Basic configuration**

   On the **Create Network Load Balancer** page, enter **Basic configuration** information such as **Name**, **Scheme**, and **IP address type**\. 

1. **Network and security**

   1. For **VPC**, select the same VPC that you used for your Application Load Balancer target\. If you selected **Internet\-facing** for **Scheme**, only VPCs with an internet gateway are available for selection\.

   1. For **Mappings**, select one or more Availability Zones and corresponding subnets\. We recommend that you select the same Availability Zones as your Application Load Balancer target to optimize availability, scaling, and performance\.

      \(Optional\) To use static IP addresses, choose **Use an Elastic IP address** in the **IPv4 settings** for each Availability Zone\. With static IP addresses you can add certain IPs to an allow list for firewalls, or you can hardcode IPs with clients\. 

1. **Listeners and routing**

   1. For **Listeners**, the default is a listener that accepts TCP traffic on port 80\. Only TCP listeners can forward traffic to an Application Load Balancer target group\. Keep the listener protocol set to TCP, but you can modify the port as required\. 

      This setup allows you to use HTTPS listeners on the Application Load Balancer to terminate the TLS protocol\.

   1. For **Default action**, select the Application Load Balancer target group that you want to forward traffic to\. If you don't see it in the list, or cannot select a target group \(because it is already in use by another Network Load Balancer\), you can create a new Application Load Balancer target group as shown in [Step 2: Create the target group with the Application Load Balancer as the target](#register-application-load-balancer-target)\. 

1. Add tags \(optional\), review your configuration, and choose **Create load balancer**\. 

**Important**  
You can associate an Application Load Balancer as a target of a maximum of two Network Load Balancers\. To do this, the Application Load Balancer must reside in separate target groups, and be assigned to two different Network Load Balancers\.

Note that each Application Load Balancer you put behind a Network Load Balancer decreases the maximum number of targets by 50 \(if cross\-zone load balancing is disabled\) or 100 \(if cross\-zone load balancing is enabled\)\. We recommend keeping cross\-zone load balancing disabled to minimize latency and avoid regional data transfer charges\. Refer to [Quotas for your Network Load Balancers](load-balancer-limits.md) for baseline limits\.

**To create the Network Load Balancer using the AWS CLI**  
Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command\.

## Step 4 \(Optional\): Enable VPC endpoint services \(AWS PrivateLink\)<a name="enable-privatelink"></a>

To use the Network Load Balancer that you set up in the previous step as an endpoint for private connectivity, you can enable AWS PrivateLink\. This establishes a private connection to your load balancer as an endpoint service\. 

****To enable AWS PrivateLink on your Network Load Balancer****

1. On the navigation pane, under **Load Balancing**, choose **Load Balancers**\. 

1. On the load balancers list page, select the Network Load Balancer to enable AWS PrivateLink\. 

1. In the load balancer details section \(below the list\), choose the **Integrated services** tab\. 

1. Scroll down to **VPC Endpoint Services \(AWS PrivateLink\)**\.

1. Choose **Create Endpoint Services**\. For the remaining steps, see [Create a VPC endpoint service configuration for interface endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html) in the *AWS PrivateLink Guide*\.