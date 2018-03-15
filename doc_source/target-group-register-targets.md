# Register Targets with Your Target Group<a name="target-group-register-targets"></a>

You register your targets with one or more target groups\. Each target group must have at least one registered target in each Availability Zone that is enabled for the load balancer\. You can register targets by instance ID or by IP address\. For more information, see [Target Groups for Your Network Load Balancers](load-balancer-target-groups.md)\.

If demand on your currently registered targets increases, you can register additional targets in order to handle the demand\. When your target is ready to handle requests, register it with your target group\. The load balancer starts routing requests to the target as soon as the registration process completes and the target passes the initial health checks\.

If demand on your registered targets decreases, or you need to service a target, you can deregister it from your target group\. The load balancer stops routing requests to a target as soon as you deregister it\. When the target is ready to receive requests, you can register it with the target group again\.

When you deregister a target, Elastic Load Balancing waits until in\-flight requests have completed\. This is known as *connection draining*\. The status of a target is `draining` while connection draining is in progress\.

If you are registering targets by instance ID, you can use your load balancer with an Auto Scaling group\. After you attach a target group to an Auto Scaling group and the group scales out, the instances launched by the Auto Scaling group are automatically registered with the target group\. If you detach the load balancer from the Auto Scaling group, the instances are automatically deregistered from the target group\. For more information, see [Attaching a Load Balancer to Your Auto Scaling Group](http://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Target Security Groups<a name="target-security-groups"></a>

When you register EC2 instances as targets, you must ensure that the security groups for these instances allow traffic on both the listener port and the health check port\.

**Limits**

+ Network Load Balancers do not have associated security groups\. Therefore, the security groups for your targets must use IP addresses to allow traffic from the load balancer\.

+ Security groups associated with targets must reference client IP addresses and/or CIDR blocks in their ingress rules \- references to client security groups \("sg-xxxxxx"\) are not supported for traffic through the load balancer\.


**Recommended Rules**  

|  | 
| --- |
| Inbound | 
|  Source  |  Port Range  |  Comment  | 
| *Client IP addresses* | *instance listener* | Allow traffic from clients on the instance listener port | 
| *VPC CIDR* | *health check* | Allow traffic from the load balancer on the health check port | 

If you do not want to grant access to the entire VPC CIDR, you can grant access to the private IP addresses used by the load balancer nodes\. There is one IP address per load balancer subnet\. To find these addresses, use the following procedure\.

**To find the private IP addresses to whitelist**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Network Interfaces**\.

1. In the search field, type the name of your Network Load Balancer\. There is one network interface per load balancer subnet\.

1. On the **Details** tab for each network interface, copy the address from **Primary private IPv4 IP**\.

## Targets and Internet\-facing Load Balancers<a name="target-connectivity"></a>

With an Internet\-facing load balancer, targets in a private subnet must have a route to the Internet to provide connectivity\. For example, the route table for the private subnet should have a route to a NAT gateway or a bastion host\.

## Register or Deregister Targets<a name="register-deregister-targets"></a>

When you create a target group, you specify whether you must register targets by instance ID or IP address\.

**Limits**

+ You cannot register instances by instance ID if they have the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, and T1\. You can register instances of these types by IP address\.

+ You cannot register targets in a peered VPC or linked through an [AWS managed VPN](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html)\.

### Register or Deregister Targets by Instance ID<a name="register-instances"></a>

The instance must be in the `running` state when you register it\.

**To register or deregister targets by instance ID**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Targets**, **Edit**\.

1. \(Optional\) For **Registered instances**, select any instances to be deregistered and choose **Remove**\.

1. \(Optional\) For **Instances**, select any running instances to be registered, modify the default instance port as needed, and then choose **Add to registered**\.

1. Choose **Save**\.

### Register or Deregister Targets by IP Address<a name="register-ip-addresses"></a>

The IP addresses that you register must be from the subnets of the VPC for the target group, the RFC 1918 range \(10\.0\.0\.0/8, 172\.16\.0\.0/12, and 192\.168\.0\.0/16\), and the RFC 6598 range \(100\.64\.0\.0/10\)\. You cannot register publicly routable IP addresses\.

**To register or deregister targets by IP address**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group and choose **Targets**, **Edit**\.

1. To register IP addresses, choose the **Register targets** icon \(the plus sign\) in the menu bar\. For each IP address, specify the network, Availability Zone, IP address, and port, and then choose **Add to list**\. When you are finished specifying addresses, choose **Register**\.

1. To deregister IP addresses, choose the **Deregister targets** icon \(the minus sign\) in the menu bar\. If you have many registered IP addresses, you might find it helpful to add a filter or change the sort order\. Select the IP addresses and choose **Deregister**\.

1. To leave this screen, choose the **Back to target group** icon \(the back button\) in the menu bar\.

### To register or deregister targets using the AWS CLI<a name="register-cli"></a>

Use the [register\-targets](http://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to add targets and the [deregister\-targets](http://docs.aws.amazon.com/cli/latest/reference/elbv2/deregister-targets.html) command to remove targets\.
