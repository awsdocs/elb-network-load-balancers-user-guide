# Listeners for your Network Load Balancers<a name="load-balancer-listeners"></a>

Before you start using your Network Load Balancer, you must add one or more *listeners*\. A listener is a process that checks for connection requests, using the protocol and port that you configure\. The rules that you define for a listener determine how the load balancer routes requests to the targets in one or more target groups\.

For more information, see [Request routing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html#request-routing) in the *Elastic Load Balancing User Guide*\.

**Topics**
+ [Listener configuration](#listener-configuration)
+ [Listener rules](#listener-rules)
+ [Create a listener for your Network Load Balancer](create-listener.md)
+ [TLS listeners for your Network Load Balancer](create-tls-listener.md)
+ [Update a listener for your Network Load Balancer](listener-update-rules.md)
+ [Update a TLS listener for your Network Load Balancer](listener-update-certificates.md)
+ [Delete a listener for your Network Load Balancer](delete-listener.md)

## Listener configuration<a name="listener-configuration"></a>

Listeners support the following protocols and ports:
+ **Protocols**: TCP, TLS, UDP, TCP\_UDP
+ **Ports**: 1\-65535

You can use a TLS listener to offload the work of encryption and decryption to your load balancer so that your applications can focus on their business logic\. If the listener protocol is TLS, you must deploy exactly one SSL server certificate on the listener\. For more information, see [TLS listeners for your Network Load Balancer](create-tls-listener.md)\.

To support both TCP and UDP on the same port, create a TCP\_UDP listener\. The target groups for a TCP\_UDP listener must use the TCP\_UDP protocol\.

You can use WebSockets with your listeners\.

All network traffic sent to a configured listener is classified as intended traffic\. Network traffic that does not match a configured listener is classified as unintended traffic\. ICMP requests other than Type 3 are also considered unintended traffic\. Network Load Balancers drop unintended traffic without forwarding it to any targets\. TCP data packets sent to the listener port for a configured listeners that are not new connections or part of an active TCP connection are rejected with a TCP reset \(RST\)\.

## Listener rules<a name="listener-rules"></a>

When you create a listener, you specify a rule for routing requests\. This rule forwards requests to the specified target group\. To update this rule, see [Update a listener for your Network Load Balancer](listener-update-rules.md)\.