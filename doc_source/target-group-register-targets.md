# Register Targets with Your Target Group<a name="target-group-register-targets"></a>

When your target is ready to handle requests, you register it with one or more target groups\. You can register targets by instance ID or by IP address\. The load balancer starts routing requests to the target as soon as the registration process completes and the target passes the initial health checks\. It can take a few minutes for the registration process to complete and health checks to start\. For more information, see [Health Checks for Your Target Groups](target-group-health-checks.md)\.

If demand on your currently registered targets increases, you can register additional targets in order to handle the demand\. If demand on your registered targets decreases, you can deregister targets from your target group\. It can take a few minutes for the deregistration process to complete and for the load balancer to stop routing requests to the target\. If demand increases subsequently, you can register targets that you deregistered with the target group again\. If you need to service a target, you can deregister it and then register it again when servicing is complete\.

When you deregister a target, Elastic Load Balancing waits until in\-flight requests have completed\. This is known as *connection draining*\. The status of a target is `draining` while connection draining is in progress\. After deregistration is complete, status of the target changes to `unused`\. For more information, see [Deregistration Delay](load-balancer-target-groups.md#deregistration-delay)\.

If you are registering targets by instance ID, you can use your load balancer with an Auto Scaling group\. After you attach a target group to an Auto Scaling group and the group scales out, the instances launched by the Auto Scaling group are automatically registered with the target group\. If you detach the load balancer from the Auto Scaling group, the instances are automatically deregistered from the target group\. For more information, see [Attaching a Load Balancer to Your Auto Scaling Group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Target Security Groups<a name="target-security-groups"></a>

When you register EC2 instances as targets, you must ensure that the security groups for these instances allow traffic on both the listener port and the health check port\.

**Limits**
+ Network Load Balancers do not have associated security groups\. Therefore, the security groups for your targets must use IP addresses to allow traffic from the load balancer\.
+ You cannot allow traffic from clients to targets through the load balancer using the security groups for the clients in the security groups for the targets\. Use the client CIDR blocks in the target security groups instead\.


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

## Network ACLs<a name="network-acls"></a>

The default network access control list \(ACL\) for a VPC allows all inbound and outbound traffic\. If you create custom network ACLs for the subnets for your instances, they must allow traffic\. The network ACL associated with the subnets for your instances must allow inbound traffic on the health check port and outbound traffic on the ephemeral ports \(1024\-65535\)\. The network ACL associated with the subnets for your load balancer nodes must allow inbound traffic on the ephemeral ports and outbound traffic on the health check and ephemeral ports\.

## Register or Deregister Targets<a name="register-deregister-targets"></a>

Each target group must have at least one registered target in each Availability Zone that is enabled for the load balancer\.

The target type of your target group determines how you register targets with that target group\. For more information, see [Target Type](load-balancer-target-groups.md#target-type)\.

**Requirements**
+ You cannot register instances by instance ID if they have the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, and T1\. You can register instances of these types by IP address\.
+ You cannot register instances by instance ID if they are in a VPC that is peered to the load balancer VPC\. You can register these instances by IP address\.
+ If you register a target by IP address and the IP address is in the same VPC as the load balancer, the load balancer verifies that it is from a subnet that it can reach\.

**Topics**
+ [Register or Deregister Targets by Instance ID](#register-instances)
+ [Register or Deregister Targets by IP Address](#register-ip-addresses)
+ [Register or Deregister Targets Using the AWS CLI](#register-cli)

### Register or Deregister Targets by Instance ID<a name="register-instances"></a>

An instance must be in the `running` state when you register it\.

**To register or deregister targets by instance ID**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Targets**, **Edit**\.

1. \(Optional\) For **Registered instances**, select any instances to be deregistered and choose **Remove**\.

1. \(Optional\) For **Instances**, select any running instances to be registered, modify the default instance port as needed, and then choose **Add to registered**\.

1. Choose **Save**\.

### Register or Deregister Targets by IP Address<a name="register-ip-addresses"></a>

An IP address that you register must be from one of the following CIDR blocks:
+ The subnets of the VPC for the target group
+ 10\.0\.0\.0/8 \(RFC 1918\)
+ 100\.64\.0\.0/10 \(RFC 6598\)
+ 172\.16\.0\.0/12 \(RFC 1918\)
+ 192\.168\.0\.0/16 \(RFC 1918\)

**To register or deregister targets by IP address**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group and choose **Targets**, **Edit**\.

1. To register IP addresses, choose the **Register targets** icon \(the plus sign\) in the menu bar\. For each IP address, specify the network, Availability Zone, IP address, and port, and then choose **Add to list**\. When you are finished specifying addresses, choose **Register**\.

1. To deregister IP addresses, choose the **Deregister targets** icon \(the minus sign\) in the menu bar\. If you have many registered IP addresses, you might find it helpful to add a filter or change the sort order\. Select the IP addresses and choose **Deregister**\.

1. To leave this screen, choose the **Back to target group** icon \(the back button\) in the menu bar\.

### Register or Deregister Targets Using the AWS CLI<a name="register-cli"></a>

Use the [register\-targets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to add targets and the [deregister\-targets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/deregister-targets.html) command to remove targets\.