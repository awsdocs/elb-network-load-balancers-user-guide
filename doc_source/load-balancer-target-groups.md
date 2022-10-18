# Target groups for your Network Load Balancers<a name="load-balancer-target-groups"></a>

Each *target group* is used to route requests to one or more registered targets\. When you create a listener, you specify a target group for its default action\. Traffic is forwarded to the target group specified in the listener rule\. You can create different target groups for different types of requests\. For example, create one target group for general requests and other target groups for requests to the microservices for your application\. For more information, see [Network Load Balancer components](introduction.md#network-load-balancer-components)\.

You define health check settings for your load balancer on a per target group basis\. Each target group uses the default health check settings, unless you override them when you create the target group or modify them later on\. After you specify a target group in a rule for a listener, the load balancer continually monitors the health of all targets registered with the target group that are in an Availability Zone enabled for the load balancer\. The load balancer routes requests to the registered targets that are healthy\. For more information, see [Health checks for your target groups](target-group-health-checks.md)\.

**Topics**
+ [Routing configuration](#target-group-routing-configuration)
+ [Target type](#target-type)
+ [IP address type](#target-group-ip-address-type)
+ [Registered targets](#registered-targets)
+ [Target group attributes](#target-group-attributes)
+ [Client IP preservation](#client-ip-preservation)
+ [Deregistration delay](#deregistration-delay)
+ [Proxy protocol](#proxy-protocol)
+ [Sticky sessions](#sticky-sessions)
+ [Create a target group for your Network Load Balancer](create-target-group.md)
+ [Health checks for your target groups](target-group-health-checks.md)
+ [Register targets with your target group](target-group-register-targets.md)
+ [Application Load Balancers as targets](application-load-balancer-target.md)
+ [Tags for your target group](target-group-tags.md)
+ [Delete a target group](delete-target-group.md)

## Routing configuration<a name="target-group-routing-configuration"></a>

By default, a load balancer routes requests to its targets using the protocol and port number that you specified when you created the target group\. Alternatively, you can override the port used for routing traffic to a target when you register it with the target group\.

Target groups for Network Load Balancers support the following protocols and ports:
+ **Protocols**: TCP, TLS, UDP, TCP\_UDP
+ **Ports**: 1\-65535

If a target group is configured with the TLS protocol, the load balancer establishes TLS connections with the targets using certificates that you install on the targets\. The load balancer does not validate these certificates\. Therefore, you can use self\-signed certificates or certificates that have expired\. Because the load balancer is in a virtual private cloud \(VPC\), traffic between the load balancer and the targets is authenticated at the packet level, so it is not at risk of man\-in\-the\-middle attacks or spoofing even if the certificates on the targets are not valid\.

The following table summarizes the supported combinations of listener protocol and target group settings\.


| Listener protocol | Target group protocol | Target group type | Health check protocol | 
| --- | --- | --- | --- | 
|  TCP  |  TCP \| TCP\_UDP  |  instance \| ip   |  HTTP \| HTTPS \| TCP  | 
|  TCP  |  TCP  |   alb  |  HTTP \| HTTPS   | 
|  TLS  |  TCP \| TLS  |  instance \| ip   |  HTTP \| HTTPS \| TCP  | 
|  UDP  |  UDP \| TCP\_UDP  |  instance \| ip   |  HTTP \| HTTPS \| TCP  | 
|  TCP\_UDP  |  TCP\_UDP  |  instance \| ip   |  HTTP \| HTTPS \| TCP  | 

## Target type<a name="target-type"></a>

When you create a target group, you specify its target type, which determines how you specify its targets\. After you create a target group, you cannot change its target type\.

The following are the possible target types:

`instance`  
The targets are specified by instance ID\.

`ip`  
The targets are specified by IP address\.

`alb`  
The target is an Application Load Balancer\.

When the target type is `ip`, you can specify IP addresses from one of the following CIDR blocks:
+ The subnets of the VPC for the target group
+ 10\.0\.0\.0/8 \([RFC 1918](https://tools.ietf.org/html/rfc1918)\)
+ 100\.64\.0\.0/10 \([RFC 6598](https://tools.ietf.org/html/rfc6598)\)
+ 172\.16\.0\.0/12 \(RFC 1918\)
+ 192\.168\.0\.0/16 \(RFC 1918\)

**Important**  
You can't specify publicly routable IP addresses\.

All of the supported CIDR blocks enable you to register the following targets with a target group: 
+ ClassicLink instances\. For more information, see [ClassicLink](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html), in the *Amazon EC2 User Guide for Linux Instances*\.
+ AWS resources that are addressable by IP address and port \(for example, databases\)\.
+ On\-premises resources linked to AWS through AWS Direct Connect or a Site\-to\-Site VPN connection\.

When client IP preservation is disabled for your target groups, the load balancer can support about 55,000 connections per minute for each combination of Network Load Balancer IP address and unique target \(IP address and port\)\. If you exceed these connections, there is an increased chance of port allocation errors\. If you get port allocation errors, add more targets to the target group\.

When launching a Network Load Balancer in a shared Amazon VPC \(as a participant\), you can only register targets in subnets that have been shared with you\.

When the target type is `alb`, you can register a single Application Load Balancer as a target\. For more information, see [Application Load Balancers as targets](application-load-balancer-target.md)\.

Network Load Balancers do not support the `lambda` target type\. Application Load Balancers are the only load balancers that support the `lambda` target type\. For more information, see [Lambda functions as targets](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/lambda-functions.html) in the *User Guide for Application Load Balancers*\.

If you have microservices on instances that are registered with a Network Load Balancer, you cannot use the load balancer to provide communication between them unless the load balancer is internet\-facing or the instances are registered by IP address\. For more information, see [Connections time out for requests from a target to its load balancer](load-balancer-troubleshooting.md#loopback-timeout)\.

### Request routing and IP addresses<a name="request-routing-ip-addresses"></a>

If you specify targets using an instance ID, traffic is routed to instances using the primary private IP address that is specified in the primary network interface for the instance\. The load balancer rewrites the destination IP address from the data packet before forwarding it to the target instance\.

If you specify targets using IP addresses, you can route traffic to an instance using any private IP address from one or more network interfaces\. This enables multiple applications on an instance to use the same port\. Note that each network interface can have its own security group\. The load balancer rewrites the destination IP address before forwarding it to the target\.

For more information about allowing traffic to your instances, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

## IP address type<a name="target-group-ip-address-type"></a>

When creating a new target group, you can select the IP address type of your target group\. This controls the IP version used to communicate with targets and check their health status\. 

Network Load Balancers support both IPv4 and IPv6 target groups\. The default selection is IPv4\. IPv6 target groups can only be associated with dualstack Network Load Balancers\.

**Considerations**
+ All IP addresses within a target group must have the same IP address type\. For example, you can't register an IPv4 target with an IPv6 target group\.
+ IPv6 target groups can only be used with `dualstack` load balancers with TCP or a TLS listeners\.
+ IPv6 target groups only support IP type targets\.

## Registered targets<a name="registered-targets"></a>

Your load balancer serves as a single point of contact for clients and distributes incoming traffic across its healthy registered targets\. Each target group must have at least one registered target in each Availability Zone that is enabled for the load balancer\. You can register each target with one or more target groups\.

If demand on your application increases, you can register additional targets with one or more target groups in order to handle the demand\. The load balancer starts routing traffic to a newly registered target as soon as the registration process completes\.

If demand on your application decreases, or if you need to service your targets, you can deregister targets from your target groups\. Deregistering a target removes it from your target group, but does not affect the target otherwise\. The load balancer stops routing traffic to a target as soon as it is deregistered\. The target enters the `draining` state until in\-flight requests have completed\. You can register the target with the target group again when you are ready for it to resume receiving traffic\.

If you are registering targets by instance ID, you can use your load balancer with an Auto Scaling group\. After you attach a target group to an Auto Scaling group, Auto Scaling registers your targets with the target group for you when it launches them\. For more information, see [Attaching a load balancer to your Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html) in the *Amazon EC2 Auto Scaling User Guide*\.

**Considerations**
+ You cannot register instances by instance ID if they use one of the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, or T1\.
+ You cannot register instances by instance ID if they are in a VPC that is peered to the load balancer VPC \(same Region or different Region\)\. You can register these instances by IP address\.
+ If you register a target by IP address and the IP address is in the same VPC as the load balancer, the load balancer verifies that it is from a subnet that it can reach\.
+ The load balancer routes traffic to targets only in Availability Zones that are enabled\. Targets in zones that are not enabled are unused\.
+ For UDP and TCP\_UDP target groups, do not register instances by IP address if they reside outside of the load balancer VPC or if they use one of the following instance types: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, or T1\. Targets that reside outside the load balancer VPC or use an unsupported instance type might be able to receive traffic from the load balancer but then be unable to respond\.

## Target group attributes<a name="target-group-attributes"></a>

The following are the target group attributes:

`deregistration_delay.timeout_seconds`  
The amount of time for Elastic Load Balancing to wait before changing the state of a deregistering target from `draining` to `unused`\. The range is 0\-3600 seconds\. The default value is 300 seconds\.

`deregistration_delay.connection_termination.enabled`  
Indicates whether the load balancer terminates connections at the end of the deregistration timeout\. The value is `true` or `false`\. The default is `false`\.

`preserve_client_ip.enabled`  
Indicates whether client IP preservation is enabled\. The value is `true` or `false`\. The default is disabled if the target group type is IP address and the target group protocol is TCP or TLS\. Otherwise, the default is enabled\. Client IP preservation cannot be disabled for UDP and TCP\_UDP target groups\.

`proxy_protocol_v2.enabled`  
Indicates whether proxy protocol version 2 is enabled\. By default, proxy protocol is disabled\.

`stickiness.enabled`  
Indicates whether sticky sessions are enabled\.

`stickiness.type`  
The type of stickiness\. The possible value is `source_ip`\.

## Client IP preservation<a name="client-ip-preservation"></a>

Network Load Balancers can preserve the source IP address of clients when routing requests to backend targets\. When you disable client IP preservation, the private IP address of the Network Load Balancer becomes the client IP address for all incoming traffic\.

By default, client IP preservation is enabled \(and cannot be disabled\) for instance and IP type target groups with UDP and TCP\_UDP protocols\. However, you can enable or disable client IP preservation for TCP and TLS target groups using the `preserve_client_ip.enabled` target group attribute\.

**Default settings**
+ Instance type target groups: Enabled
+ IP type target groups \(UDP, TCP\_UDP\): Enabled
+ IP type target groups \(TCP, TLS\): Disabled

**Considerations**
+ When client IP preservation is enabled, targets must be in the same VPC as the Network Load Balancer, and traffic must flow directly from the Network Load Balancer to the target\.
+ Client IP preservation is not supported when traffic is routed through a Gateway Load Balancer endpoint, even if the target is in the same VPC as the Network Load Balancer\.
+ The following instance types do not support client IP preservation: C1, CC1, CC2, CG1, CG2, CR1, G1, G2, HI1, HS1, M1, M2, M3, and T1\. We recommend that you register these instance types as IP addresses with client IP preservation disabled\.
+ Client IP preservation has no effect on AWS PrivateLink traffic\. The source IP of the AWS PrivateLink traffic is always the private IP address of the Network Load Balancer\.
+ Client IP preservation has no effect on traffic converted from IPv6 to IPv4\. The source IP of this type of traffic is always the private IP address of the Network Load Balancer\.
+ When you specify targets by Application Load Balancer type, the client IP of all incoming traffic is preserved by the Network Load Balancer and is sent to the Application Load Balancer\. The Application Load Balancer then appends the client IP to the `X-Forwarded-For` request header before sending it to the target\.
+ Client IP preservation changes take effect only for new TCP connections\.
+ When client IP preservation is enabled, you might encounter TCP/IP connection limitations related to observed socket reuse on the targets\. These connection limitations can occur when a client, or a NAT device in front of the client, uses the same source IP address and source port when connecting to multiple load balancer nodes simultaneously\. If the load balancer routes these connections to the same target, the connections appear to the target as if they come from the same source socket, which results in connection errors\. If this happens, the clients can retry \(if the connection fails\) or reconnect \(if the connection is interrupted\)\. You can reduce this type of connection error by increasing the number of source ephemeral ports or by increasing the number of targets for the load balancer\. You can prevent this type of connection error, by disabling client IP preservation or disabling cross\-zone load balancing\.
+ Your Network Load Balancer supports 55,000 simultaneous connections or about 55,000 connections per minute to each unique target \(IP address and port\)\. If you exceed these connections, there is an increased chance of port allocation errors, resulting in failures to establish new connections\. Port allocation errors can be tracked using the `PortAllocationErrorCount` metric\. To fix port allocation errors, add more targets to the target group\. For more information, see [CloudWatch metrics for your Network Load Balancer](load-balancer-cloudwatch-metrics.md)\.

------
#### [ New console ]

**To configure client IP preservation using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose the name of the target group to open its details page\.

1. On the **Attributes** tab, choose **Edit**\.

1. To enable client IP preservation, select **Preserve client IP addresses**\. To disable client IP preservation, clear **Preserve client IP addresses**\.

1. Choose **Save changes**\.

------
#### [ Old console ]

**To configure client IP preservation using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group and choose **Description**, **Edit attributes**\.

1. To enable client IP preservation, select **Preserve client IP addresses**\. To disable client IP preservation, clear **Preserve client IP addresses**\.

1. Choose **Save**\.

------

**To enable or disable client IP preservation using the AWS CLI**  
Use the [modify\-target\-group\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command with the `preserve_client_ip.enabled` attribute\.

For example, use the following command to disable client IP preservation\.

```
aws elbv2 modify-target-group-attributes --attributes Key=preserve_client_ip.enabled,Value=false --target-group-arn ARN
```

Your output should be similar to the following example\.

```
{
    "Attributes": [
      {
        "Key": "proxy_protocol_v2.enabled", 
        "Value": "false"
      }, 
      {
        "Key": "preserve_client_ip.enabled", 
        "Value": "false" 
      },
      {
        "Key": "deregistration_delay.timeout_seconds", 
        "Value": "300"
      } 
    ]
}
```

## Deregistration delay<a name="deregistration-delay"></a>

When you deregister a target, the load balancer stops creating new connections to the target\. The load balancer uses connection draining to ensure that in\-flight traffic completes on the existing connections\. If the deregistered target stays healthy and an existing connection is not idle, the load balancer can continue to send traffic to the target\. To ensure that existing connections are closed, you can do one of the following: enable the target group attribute for connection termination, ensure that the instance is unhealthy before you deregister it, or periodically close client connections\.

The initial state of a deregistering target is `draining`\. By default, the load balancer changes the state of a deregistering target to `unused` after 300 seconds\. To change the amount of time that the load balancer waits before changing the state of a deregistering target to `unused`, update the deregistration delay value\. We recommend that you specify a value of at least 120 seconds to ensure that requests are completed\.

If you enable the target group attribute for connection termination, connections to deregistered targets are closed shortly after the end of the deregistration timeout\.

------
#### [ New console ]

**To update the deregistration attributes using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Choose the name of the target group to open its details page\.

1. On **Attributes** tab, choose **Edit**\.

1. To change the deregistration timeout, enter a new value for **Deregistration delay**\. To ensure that existing connections are closed after you deregister targets, select **Connection termination on deregistration**\.

1. Choose **Save changes**\.

------
#### [ Old console ]

**To update the deregistration attributes using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group and choose **Description**, **Edit attributes**\.

1. To change the deregistration timeout, enter a new value for **Deregistration delay**\. To ensure that existing connections are closed after you deregister targets, select **Connection termination on deregistration**\.

1. Choose **Save**\.

------

**To update the deregistration attributes using the AWS CLI**  
Use the [modify\-target\-group\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command\.

## Proxy protocol<a name="proxy-protocol"></a>

Network Load Balancers use proxy protocol version 2 to send additional connection information such as the source and destination\. Proxy protocol version 2 provides a binary encoding of the proxy protocol header\. The load balancer prepends a proxy protocol header to the TCP data\. It does not discard or overwrite any existing data, including any proxy protocol headers sent by the client or any other proxies, load balancers, or servers in the network path\. Therefore, it is possible to receive more than one proxy protocol header\. Also, if there is another network path to your targets outside of your Network Load Balancer, the first proxy protocol header might not be the one from your Network Load Balancer\.

If you specify targets by IP address, the source IP addresses provided to your applications depend on the protocol of the target group as follows:
+ TCP and TLS: The source IP addresses are the private IP addresses of the load balancer nodes\. If you need the IP addresses of the clients, enable proxy protocol and get the client IP addresses from the proxy protocol header\.
+ UDP and TCP\_UDP: The source IP addresses are the IP addresses of the clients\.

If you specify targets by instance ID, the source IP addresses provided to your applications are the client IP addresses\. However, if you prefer, you can enable proxy protocol and get the client IP addresses from the proxy protocol header\.

### Health check connections<a name="health-check-connections"></a>

After you enable proxy protocol, the proxy protocol header is also included in health check connections from the load balancer\. However, with health check connections, the client connection information is not sent in the proxy protocol header\.

### VPC endpoint services<a name="custom-tlv"></a>

For traffic coming from service consumers through a [VPC endpoint service](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html), the source IP addresses provided to your applications are the private IP addresses of the load balancer nodes\. If your applications need the IP addresses of the service consumers, enable proxy protocol and get them from the proxy protocol header\.

The proxy protocol header also includes the ID of the endpoint\. This information is encoded using a custom Type\-Length\-Value \(TLV\) vector as follows\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-target-groups.html)

For an example that parses TLV type 0xEA, see [https://github\.com/aws/elastic\-load\-balancing\-tools/tree/master/proprot](https://github.com/aws/elastic-load-balancing-tools/tree/master/proprot)\.

### Enable proxy protocol<a name="enable-proxy-protocol"></a>

Before you enable proxy protocol on a target group, make sure that your applications expect and can parse the proxy protocol v2 header, otherwise, they might fail\. For more information, see [PROXY protocol versions 1 and 2](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt)\.

------
#### [ New console ]

**To enable proxy protocol v2 using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Choose the name the target group to open its details page\.

1. On the **Attributes** tab, choose **Edit**\.

1. On the **Edit attributes** page, select **Proxy protocol v2**\.

1. Choose **Save changes**\.

------
#### [ Old console ]

**To enable proxy protocol v2 using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Description**, **Edit attributes**\.

1. For **Proxy protocol v2**, choose **Enable**\.

1. Choose **Save**\.

------

**To enable proxy protocol v2 using the AWS CLI**  
Use the [modify\-target\-group\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command\.

## Sticky sessions<a name="sticky-sessions"></a>

Sticky sessions are a mechanism to route client traffic to the same target in a target group\. This is useful for servers that maintain state information in order to provide a continuous experience to clients\.

**Considerations**
+ Using sticky sessions can lead to an uneven distribution of connections and flows, which might impact the availability of your targets\. For example, all clients behind the same NAT device have the same source IP address\. Therefore, all traffic from these clients is routed to the same target\.
+ The load balancer might reset the sticky sessions for a target group if the health state of any of its targets changes or if you register or deregister targets with the target group\.
+ Sticky sessions are not supported with TLS listeners when used together with TLS target groups\.

------
#### [ New console ]

**To enable sticky sessions using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Choose the name of the target group to open its details page\.

1. On the **Attributes** tab, choose **Edit**\.

1. On the **Edit attributes** page, select **Stickiness**\.

1. Choose **Save changes**\.

------
#### [ Old console ]

**To enable sticky sessions using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Description**, **Edit attributes**\.

1. For **Stickiness**, select **Enable**\.

1. Choose **Save**\.

------

**To enable sticky sessions using the AWS CLI**  
Use the [modify\-target\-group\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command with the `stickiness.enabled` attribute\.