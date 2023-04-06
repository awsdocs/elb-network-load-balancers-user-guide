# Troubleshoot your Network Load Balancer<a name="load-balancer-troubleshooting"></a>

The following information can help you troubleshoot issues with your Network Load Balancer\.

## A registered target is not in service<a name="target-not-in-service"></a>

If a target is taking longer than expected to enter the `InService` state, it might be failing health checks\. Your target is not in service until it passes one health check\. For more information, see [Health checks for your target groups](target-group-health-checks.md)\.

Verify that your instance is failing health checks and then check for the following:

**A security group does not allow traffic**  
The security groups associated with an instance must allow traffic from the load balancer using the health check port and health check protocol\. For more information, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

**A network access control list \(ACL\) does not allow traffic**  
The network ACL associated with the subnets for your instances and the subnets for your load balancer must allow traffic and health checks from the load balancer\. For more information, see [Network ACLs](target-group-register-targets.md#network-acls)\.

## Requests are not routed to targets<a name="requests-not-routed"></a>

Check for the following:

**A security group does not allow traffic**  
The security groups associated with the instances must allow traffic on the listener port from client IP addresses \(if targets are specified by instance ID\) or load balancer nodes \(if targets are specified by IP address\)\. For more information, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

**A network access control list \(ACL\) does not allow traffic**  
The network ACLs associated with the subnets for your VPC must allow the load balancer and targets to communicate in both directions on the listener port\. For more information, see [Network ACLs](target-group-register-targets.md#network-acls)\.

**The targets are in an Availability Zone that is not enabled**  
If you register targets in an Availability Zone but do not enable the Availability Zone, these registered targets do not receive traffic from the load balancer\.

**The instance is in a peered VPC**  
If you have instances in a VPC that is peered with the load balancer VPC, you must register them with your load balancer by IP address, not by instance ID\.

## Targets receive more health check requests than expected<a name="health-check-interval"></a>

Health checks for a Network Load Balancer are distributed and use a consensus mechanism to determine target health\. Therefore, targets receive more than the number of health checks configured through the `HealthCheckIntervalSeconds` setting\.

## Targets receive fewer health check requests than expected<a name="too-few-health-checks"></a>

Check whether `net.ipv4.tcp_tw_recycle` is enabled\. This setting is known to cause issues with load balancers\. The `net.ipv4.tcp_tw_reuse` setting is considered a safer alternative\.

## Unhealthy targets receive requests from the load balancer<a name="no-healthy-targets"></a>

If there is at least one healthy registered target for your load balancer, the load balancer routes requests only to its healthy registered targets\. If there are only unhealthy registered targets, the load balancer routes requests to all registered targets\.

## Target fails HTTP or HTTPS health checks due to host header mismatch<a name="host-header-mismatch"></a>

The HTTP host header in the health check request contains the IP address of the load balancer node and the listener port, not the IP address of the target and the health check port\. If you are mapping incoming requests by host header, you must ensure that health checks match any HTTP host header\. Another option is to add a separate HTTP service on a different port and configure the target group to use that port for health checks instead\. Alternatively, consider using TCP health checks\.

## Increase in TCP\_ELB\_Reset\_Count metric<a name="elb-reset-count-metric"></a>

For each TCP request that a client makes through a Network Load Balancer, the state of that connection is tracked\. If no data is sent through the connection by either the client or the target for longer than the idle timeout, the connection is closed\. If a client or a target sends data after the idle timeout period elapses, it receives a TCP RST packet to indicate that the connection is no longer valid\. Additionally, if a target becomes unhealthy, the load balancer sends a TCP RST for packets received on the client connections associated with the target, unless the unhealthy target triggers the load balancer to fail open\.

If you see a spike in the `TCP_ELB_Reset_Count` metric just before or just as the `UnhealthyHostCount` metric increases, it is likely that the TCP RST packets were sent because the target was starting to fail but hadn't been marked unhealthy\. If you see persistent increases in `TCP_ELB_Reset_Count` without targets being marked unhealthy, you can check the VPC flow logs for clients sending data on expired flows\.

## Connections time out for requests from a target to its load balancer<a name="loopback-timeout"></a>

Check whether client IP preservation is enabled on your target group\. Load balancers with client IP preservation enabled do not support hairpinning or loopback\. If an instance is a client of a load balancer that it's registered with, and it has client IP preservation enabled, the connection succeeds only if the request is routed to a different instance\. Otherwise, the source and destination IP addresses are the same and the connection times out\.

If an instance must send requests to a load balancer that it's registered with, do one of the following:
+ Disable client IP preservation\.
+ Ensure that containers that must communicate, are on different container instances\.

## Performance decreases when moving targets to a Network Load Balancer<a name="load-balancer-performance"></a>

Both Classic Load Balancers and Application Load Balancers use connection multiplexing, but Network Load Balancers do not\. Therefore, your targets can receive more TCP connections behind a Network Load Balancer\. Be sure that your targets are prepared to handle the volume of connection requests they might receive\.

## Port allocation errors connecting through AWS PrivateLink<a name="port-allocation-errors-privatelink"></a>

If your Network Load Balancer is associated with a VPC endpoint service, it supports 55,000 simultaneous connections or about 55,000 connections per minute to each unique target \(IP address and port\)\. If you exceed these connections, there is an increased chance of port allocation errors\. Port allocation errors can be tracked using the `PortAllocationErrorCount` metric\. To fix port allocation errors, add more targets to the target group\. For more information, see [CloudWatch metrics for your Network Load Balancer](load-balancer-cloudwatch-metrics.md)\.

## Intermittent connection failure when client IP preservation is enabled<a name="intermittent-connection-failure"></a>

When client IP preservation is enabled, you might encounter TCP/IP connection limitations related to observed socket reuse on the targets\. These connection limitations can occur when a client, or a NAT device in front of the client, uses the same source IP address and source port when connecting to multiple load balancer nodes simultaneously\. If the load balancer routes these connections to the same target, the connections appear to the target as if they come from the same source socket, which results in connection errors\. If this happens, clients can retry \(if the connection fails\) or reconnect \(if the connection is interrupted\)\. You can reduce this type of connection error by increasing the number of source ephemeral ports or by increasing the number of targets for the load balancer\. You can prevent this type of connection error by disabling client IP preservation or by disabling cross\-zone load balancing\.

Additionally, when client IP preservation is enabled, connectivity might fail if the clients that are connecting to the Network Load Balancer are also connected to targets behind the load balancer\. To resolve this, you can disable client IP preservation on the affected target groups\. Alternatively, have your clients connect only to the Network Load Balancer, or only to the targets, but not both\.

## TCP connection delays<a name="tcp-delays"></a>

When both cross\-zone load balancing and client IP preservation are enabled, a client connecting to different IPs on the same load balancer may be routed to the same target\. If the client uses the same source port for both of these connections, the target will receive what appears to be a duplicate connection, which can lead to connection errors and TCP delays in establishing new connections\. You can prevent this type of connection error by disabling cross\-zone load balancing\. For more information, see [Cross\-zone load balancing](network-load-balancers.md#cross-zone-load-balancing)\. 

## Potential failure when the load balancer is being provisioned<a name="load-balancer-provision-failure"></a>

One of the reasons a Network Load Balancer could fail when it is being provisioned is if you use an IP address that is already assigned or allocated elsewhere \(for example, assigned as a secondary IP address for an EC2 instance\)\. This IP address prevents the load balancer from being set up, and its state is `failed`\. You can resolve this by de\-allocating the associated IP address and retrying the creation process\.

## Intermittent 503 errors with an Application Load Balancer as a target<a name="intermittent-503-errors-alb"></a>

You can get intermittent 503 errors while using an Application Load Balancer as a target of a Network Load Balancer if the Network Load Balancer has cross\-zone load balancing enabled and the Application Load Balancer has cross\-zone load balancing disabled\. You can prevent this issue by disabling cross\-zone load balancing on the Network Load Balancer, and enabling it on the Application Load Balancer\.