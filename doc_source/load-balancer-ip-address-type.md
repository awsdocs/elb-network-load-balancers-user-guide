# IP address types for your Network Load Balancer<a name="load-balancer-ip-address-type"></a>

You can configure your Network Load Balancer so that clients can communicate with the load balancer using IPv4 addresses only, or using both IPv4 and IPv6 addresses\. The load balancer communicates with targets using IPv4 addresses, regardless of how the client communicates with the load balancer\. For more information, see [IP address type](network-load-balancers.md#ip-address-type)\.

**IPv6 requirements**
+ An internet\-facing load balancer with the `dualstack` IP address type\. You can set the IP address type when you create the load balancer and update it at any time\.
+ The virtual private cloud \(VPC\) and subnets that you specify for the load balancer must have associated IPv6 CIDR blocks\. For more information, see [IPv6 addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html#ipv6-addressing) in the *Amazon EC2 User Guide*\.
+ The load balancer must have only TCP and TLS listeners\.
+ The route tables for the load balancer subnets must route IPv6 traffic\.
+ The network ACLs for the load balancer subnets must allow IPv6 traffic\.

**To set the IP address type at creation**  
Configure settings as described in [Create a load balancer](create-network-load-balancer.md)\.

**To update the IP address type using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Actions**, **Edit IP address type**\.

1. For **IP address type**, choose **ipv4** to support IPv4 addresses only or **dualstack** to support both IPv4 and IPv6 addresses\.

1. Choose **Save**\.

**To update the IP address type using the AWS CLI**  
Use the [set\-ip\-address\-type](https://docs.aws.amazon.com/cli/latest/reference/elbv2/set-ip-address-type.html) command\.