# Listeners for Your Network Load Balancers<a name="load-balancer-listeners"></a>

Before you start using your Network Load Balancer, you must add one or more *listeners*\. A listener is a process that checks for connection requests, using the protocol and port that you configure\. The rules that you define for a listener determine how the load balancer routes requests to the targets in one or more target groups\.

**Topics**
+ [Listener Configuration](#listener-configuration)
+ [Listener Rules](#listener-rules)
+ [Create a Listener for Your Network Load Balancer](create-listener.md)
+ [Update a Listener for Your Network Load Balancer](listener-update-rules.md)
+ [Delete a Listener for Your Network Load Balancer](delete-listener.md)

## Listener Configuration<a name="listener-configuration"></a>

Listeners support the following protocols and ports:
+ **Protocols**: TCP
+ **Ports**: 1\-65535

You can use WebSockets with your TCP listeners\.

## Listener Rules<a name="listener-rules"></a>

When you create a listener, you specify a rule for routing requests\. This rule forwards requests to the specified target group\. To update this rule, see [Update a Listener for Your Network Load Balancer](listener-update-rules.md)\.