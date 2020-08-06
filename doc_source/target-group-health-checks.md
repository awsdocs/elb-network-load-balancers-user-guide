# Health checks for your target groups<a name="target-group-health-checks"></a>

You register your targets with one or more target groups\. The load balancer starts routing requests to a newly registered target as soon as the registration process completes\. It can take a few minutes for the registration process to complete and health checks to start\.

Network Load Balancers use active and passive health checks to determine whether a target is available to handle requests\. By default, each load balancer node routes requests only to the healthy targets in its Availability Zone\. If you enable cross\-zone load balancing, each load balancer node routes requests to the healthy targets in all enabled Availability Zones\. For more information, see [Cross\-zone load balancing](network-load-balancers.md#cross-zone-load-balancing)\.

With active health checks, the load balancer periodically sends a request to each registered target to check its status\. Each load balancer node checks the health of each target, using the health check settings for the target group with which the target is registered\. After each health check is completed, the load balancer node closes the connection that was established for the health check\.

With passive health checks, the load balancer observes how targets respond to connections\. Passive health checks enable the load balancer to detect an unhealthy target before it is reported as unhealthy by the active health checks\. You cannot disable, configure, or monitor passive health checks\. Passive health checks are not supported for UDP traffic\.

If a target becomes unhealthy, the load balancer sends a TCP RST for packets received on the client connections associated with the target\.

If one or more target groups does not have a healthy target in an enabled Availability Zone, we remove the IP address for the corresponding subnet from DNS so that requests cannot be routed to targets in that Availability Zone\. If there are no enabled Availability Zones with a healthy target in each target group, requests are routed to targets in all enabled Availability Zones\.

For HTTP or HTTPS health check requests, the host header contains the IP address of the load balancer node and the listener port, not the IP address of the target and the health check port\.

If you add a TLS listener to your Network Load Balancer, we perform a listener connectivity test\. As TLS termination also terminates a TCP connection, a new TCP connection is established between your load balancer and your targets\. Therefore, you might see the TCP pings for this test sent from your load balancer to the targets that are registered with your TLS listener\. You can identify these TCP pings because they have the source IP address of your Network Load Balancer and the connections do not contain data packets\.

For a UDP service, availability is tested using TCP active health checks directed to a TCP port on your target\. You can use any TCP port on your target to verify the availability of a UDP service\. If the service listening to the health check port fails, your target is considered unavailable\. To improve the accuracy of health checks for a UDP service, configure the service listening to the health check port to track the status of your UDP service and close the health check port if the service is unavailable\.

## Health check settings<a name="health-check-settings"></a>

You configure active health checks for the targets in a target group using the following settings\. If the health checks exceed **UnhealthyThresholdCount** consecutive failures, the load balancer takes the target out of service\. When the health checks exceed **HealthyThresholdCount** consecutive successes, the load balancer puts the target back in service\.


| Setting | Description | 
| --- | --- | 
| **HealthCheckProtocol** |  The protocol the load balancer uses when performing health checks on targets\. The possible protocols are HTTP, HTTPS, and TCP\. The default is the TCP protocol\.  | 
| **HealthCheckPort** |  The port the load balancer uses when performing health checks on targets\. The default is to use the port on which each target receives traffic from the load balancer\.  | 
| **HealthCheckPath** |  \[HTTP/HTTPS health checks\] The ping path that is the destination on the targets for health checks\. The default is /\.  | 
| **HealthCheckTimeoutSeconds** |  The amount of time, in seconds, during which no response from a target means a failed health check\. This value must be 6 seconds for HTTP health checks and 10 seconds for TCP and HTTPS health checks\.  | 
| **HealthCheckIntervalSeconds** |  The approximate amount of time, in seconds, between health checks of an individual target\. This value can be 10 seconds or 30 seconds\. The default is 30 seconds\.  Health checks for a Network Load Balancer are distributed and use a consensus mechanism to determine target health\. Therefore, targets receive more than the configured number of health checks\. To reduce the impact to your targets if you are using HTTP health checks, use a simpler destination on the targets, such as a static HTML file, or switch to TCP health checks\.   | 
| **HealthyThresholdCount** |  The number of consecutive successful health checks required before considering an unhealthy target healthy\. The range is 2 to 10\. The default is 3\.  | 
| **UnhealthyThresholdCount** |  The number of consecutive failed health checks required before considering a target unhealthy\. This value must be the same as the healthy threshold count\.  | 
| **Matcher** |  \[HTTP/HTTPS health checks\] The HTTP codes to use when checking for a successful response from a target\. This value must be 200 to 399\.  | 

## Target health status<a name="target-health-states"></a>

Before the load balancer sends a health check request to a target, you must register it with a target group, specify its target group in a listener rule, and ensure that the Availability Zone of the target is enabled for the load balancer\.

The following table describes the possible values for the health status of a registered target\.


| Value | Description | 
| --- | --- | 
| `initial` |  The load balancer is in the process of registering the target or performing the initial health checks on the target\. Related reason codes: `Elb.RegistrationInProgress` \| `Elb.InitialHealthChecking`  | 
| `healthy` |  The target is healthy\. Related reason codes: None  | 
| `unhealthy` |  The target did not respond to a health check or failed the health check\. Related reason code: `Target.FailedHealthChecks`  | 
| `unused` |  The target is not registered with a target group, the target group is not used in a listener rule, the target is in an Availability Zone that is not enabled, or the target is in the stopped or terminated state\. Related reason codes: `Target.NotRegistered` \| `Target.NotInUse` \| `Target.InvalidState` \| `Target.IpUnusable`  | 
| `draining` |  The target is deregistering and connection draining is in process\. Related reason code: `Target.DeregistrationInProgress`  | 
| `unavailable` |  Target health is unavailable\. Related reason code: `Elb.InternalError`  | 

## Health check reason codes<a name="target-health-reason-codes"></a>

If the status of a target is any value other than `Healthy`, the API returns a reason code and a description of the issue, and the console displays the same description in a tooltip\. Note that reason codes that begin with `Elb` originate on the load balancer side and reason codes that begin with `Target` originate on the target side\.


| Reason code | Description | 
| --- | --- | 
| `Elb.InitialHealthChecking` |  Initial health checks in progress  | 
| `Elb.InternalError` |  Health checks failed due to an internal error  | 
| `Elb.RegistrationInProgress` |  Target registration is in progress  | 
| `Target.DeregistrationInProgress` |  Target deregistration is in progress  | 
| `Target.FailedHealthChecks` |  Health checks failed  | 
| `Target.InvalidState` |  Target is in the stopped state Target is in the terminated state Target is in the terminated or stopped state Target is in an invalid state  | 
| `Target.IpUnusable` |  The IP address cannot be used as a target, as it is in use by a load balancer  | 
| `Target.NotInUse` |  Target group is not configured to receive traffic from the load balancer Target is in an Availability Zone that is not enabled for the load balancer  | 
| `Target.NotRegistered` |  Target is not registered to the target group  | 

## Check the health of your targets<a name="check-target-health"></a>

You can check the health status of the targets registered with your target groups\.

------
#### [ New console ]

**To check the health of your targets using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Choose the name of the target group to open its details page\.

1. On the **Targets** tab, the **Status** column indicates the status of each target\.

1. If the target status is any value other than `Healthy`, the **Status details** column contains more information\.

------
#### [ Old console ]

**To check the health of your targets using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Targets**, and view the status of each target in the **Status** column\. If the status is any value other than `Healthy`, view the tooltip for more information\.

------

**To check the health of your targets using the AWS CLI**  
Use the [describe\-target\-health](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-target-health.html) command\. The output of this command contains the target health state\. It includes a reason code if the status is any value other than `Healthy`\.

**To receive email notifications about unhealthy targets**  
Use CloudWatch alarms to trigger a Lambda function to send details about unhealthy targets\. For step\-by\-step instructions, see the following blog post: [Identifying unhealthy targets of your load balancer](http://aws.amazon.com/blogs/networking-and-content-delivery/identifying-unhealthy-targets-of-elastic-load-balancer/)\.

## Modify the health check settings of a target group<a name="modify-health-check-settings"></a>

You can modify some of the health check settings for your target group\. If the protocol of the target group is TCP, TLS, UDP, or TCP\_UDP, you can't modify the health check protocol, interval, timeout, or success codes\.

------
#### [ New console ]

**To modify health check settings for a target group using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Choose the name of the target group to open its details page\.

1. On the **Group details** tab, in the **Health check settings** section, choose **Edit**\.

1. On the **Edit health check settings** page, modify the settings as needed, and then choose **Save changes**\.

------
#### [ Old console ]

**To modify health check settings for a target group using the old console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Target Groups**\.

1. Select the target group\.

1. Choose **Health checks**, **Edit**\.

1. On the **Edit target group** page, modify the settings as needed, and then choose **Save**\.

------

**To modify health check settings for a target group using the AWS CLI**  
Use the [modify\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group.html) command\.