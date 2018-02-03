# Target Groups for Your Network Load Balancers<a name="load-balancer-target-groups"></a>

Each *target group* is used to route requests to one or more registered targets\. When you create each listener rule, you specify a target group and conditions\. When a rule condition is met, traffic is forwarded to the corresponding target group\. You can create different target groups for different types of requests\. For example, create one target group for general requests and other target groups for requests to the microservices for your application\. For more information, see [Network Load Balancer Components](introduction.md#network-load-balancer-components)\.

You define health check settings for your load balancer on a per target group basis\. Each target group uses the default health check settings, unless you override them when you create the target group or modify them later on\. After you specify a target group in a rule for a listener, the load balancer continually monitors the health of all targets registered with the target group that are in an Availability Zone enabled for the load balancer\. The load balancer routes requests to the registered targets that are healthy\.


+ [Routing Configuration](#target-group-routing-configuration)
+ [Target Type](#target-type)
+ [Registered Targets](#registered-targets)
+ [Target Group Attributes](#target-group-attributes)
+ [Deregistration Delay](#deregistration-delay)
+ [Proxy Protocol](#proxy-protocol)
+ [Create a Target Group for Your Network Load Balancer](create-target-group.md)
+ [Health Checks for Your Target Groups](target-group-health-checks.md)
+ [Register Targets with Your Target Group](target-group-register-targets.md)
+ [Tags for Your Target Group](target-group-tags.md)
+ [Delete a Target Group](delete-target-group.md)

## Routing Configuration<a name="target-group-routing-configuration"></a>

By default, a load balancer routes requests to its targets using the protocol and port number that you specified when you created the target group\. Alternatively, you can override the port used for routing traffic to a target when you register it with the target group\.

Target groups for Network Load Balancers support the following protocols and ports:

+ **Protocols**: TCP

+ **Ports**: 1\-65535

## Target Type<a name="target-type"></a>

When you create a target group, you specify its target type, which determines how you specify its targets\. After you create a target group, you cannot change its target type\.

The following are the possible target types:

`instance`  
The targets are specified by instance ID\.

`ip`  
The targets are specified by IP address\.

When the target type is `ip`, you can specify IP addresses from one of the following CIDR blocks:

+ The subnets of the VPC for the target group

+ 10\.0\.0\.0/8 \(RFC 1918\)

+ 100\.64\.0\.0/10 \(RFC 6598\)

+ 172\.16\.0\.0/12 \(RFC 1918\)

+ 192\.168\.0\.0/16 \(RFC 1918\)

These supported CIDR blocks enable you to register the following with a target group: ClassicLink instances, AWS resources that are addressable by IP address and port \(for example, databases\), and on\-premises resources linked to AWS through AWS Direct Connect or a software VPN connection\.

**Important**  
You can't specify publicly routable IP addresses\.

If you specify targets using an instance ID, traffic is routed to instances using the primary private IP address specified in the primary network interface for the instance\. If you specify targets using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces\. This enables multiple applications on an instance to use the same port\. Note that each network interface can have its own security group\.

If you specify targets using an instance ID, the source IP addresses of the clients are preserved and provided to your applications\. If you specify targets by IP address, the source IP addresses are the private IP addresses of the load balancer nodes\.

## Registered Targets<a name="registered-targets"></a>

Your load balancer serves as a single point of contact for clients and distributes incoming traffic across its healthy registered targets\. Each target group must have at least one registered target in each Availability Zone that is enabled for the load balancer\. You can register each target with one or more target groups\. You can register each EC2 instance or IP address with the same target group multiple times using different ports, which enables the load balancer to route requests to microservices\.

If demand on your application increases, you can register additional targets with one or more target groups in order to handle the demand\. The load balancer starts routing traffic to a newly registered target as soon as the registration process completes and the target passes the initial health checks\.

If demand on your application decreases, or you need to service your targets, you can deregister targets from your target groups\. Deregistering a target removes it from your target group, but does not affect the target otherwise\. The load balancer stops routing traffic to a target as soon as it is deregistered\. The target enters the `draining` state until in\-flight requests have completed\. You can register the target with the target group again when you are ready for it to resume receiving traffic\.

If you are registering targets by instance ID, you can use your load balancer with an Auto Scaling group\. After you attach a target group to an Auto Scaling group, Auto Scaling registers your targets with the target group for you when it launches them\. For more information, see [Attaching a Load Balancer to Your Auto Scaling Group](http://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) in the *Amazon EC2 Auto Scaling User Guide*\.

**Limits**

+ You cannot register instances by instance ID if they have the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, and T1\. You can register instances of these types by IP address\.

+ You cannot register targets in a peered VPC or linked through an [AWS managed VPN](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html)\.

## Target Group Attributes<a name="target-group-attributes"></a>

The following are the target group attributes:

`deregistration_delay.timeout_seconds`  
The amount of time for Elastic Load Balancing to wait before changing the state of a deregistering target from `draining` to `unused`\. The range is 0\-3600 seconds\. The default value is 300 seconds\.

`proxy_protocol_v2.enabled`  
Indicates whether Proxy Protocol version 2 is enabled\. By default, Proxy Protocol is disabled\.

## Deregistration Delay<a name="deregistration-delay"></a>

Elastic Load Balancing stops sending requests to instances that are deregistering\. Connection draining ensures that in\-flight requests complete before existing connections are closed\. The initial state of a deregistering target is `draining`\. By default, the state of a deregistering target changes to `unused` after 300 seconds\. To change the amount of time that Elastic Load Balancing waits before changing the state to `unused`, update the deregistration delay value\.

**To update the deregistration delay value using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Description**, **Edit attributes**\.

1. Change the value of **Deregistration delay** as needed, and then choose **Save**\.

**To update the deregistration delay value using the AWS CLI**  
Use the [modify\-target\-group\-attributes](http://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command\.

## Proxy Protocol<a name="proxy-protocol"></a>

Network Load Balancers use Proxy Protocol version 2 to send additional connection information such as the source and destination\. Proxy Protocol version 2 provides a binary encoding of the Proxy Protocol header\.

If you specify targets by IP address, the source IP addresses provided to your applications are the private IP addresses of the load balancer nodes\. If your applications need the IP addresses of the clients, enable Proxy Protocol and get the client IP addresses from the Proxy Protocol header\.

If you specify targets by instance ID, the source IP addresses provided to your applications are the client IP addresses\. However, if you prefer, you can enable Proxy Protocol and get the client IP addresses from the Proxy Protocol header\.

### Health Check Connections<a name="health-check-connections"></a>

After you enable Proxy Protocol, the Proxy Protocol header is also included in health check connections from the load balancer\. However, with health check connections, the client connection information is not sent in the Proxy Protocol header\.

### VPC Endpoint Services<a name="custom-tlv"></a>

For traffic coming from service consumers through a [VPC endpoint service](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/endpoint-service.html), the source IP addresses provided to your applications are the private IP addresses of the load balancer nodes\. If your applications need the IP addresses of the service consumers, enable Proxy Protocol and get them from the Proxy Protocol header\.

The Proxy Protocol header also includes the ID of the endpoint\. This information is encoded using a custom Type\-Length\-Value \(TLV\) vector as follows\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-target-groups.html)

For an example that parses TLV type 0xEA, see [https://github\.com/aws/elastic\-load\-balancing\-tools/tree/master/proprot](https://github.com/aws/elastic-load-balancing-tools/tree/master/proprot)\.

### Enable Proxy Protocol<a name="enable-proxy-protocol"></a>

Before you enable Proxy Protocol on a target group, make sure that your applications expect and can parse the Proxy Protocol v2 header, otherwise, they might fail\. For more information, see [PROXY protocol versions 1 and 2](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt)\.

**To enable Proxy Protocol using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Description**, **Edit attributes**\.

1. Select **Enable proxy protocol v2**, and then choose **Save**\.

**To enable Proxy Protocol using the AWS CLI**  
Use the [modify\-target\-group\-attributes](http://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command\.