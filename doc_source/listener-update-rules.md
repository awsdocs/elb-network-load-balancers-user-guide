# Update a listener for your Network Load Balancer<a name="listener-update-rules"></a>

You can update the listener port, listener protocol, or the default listener rule\. The default listener rule forwards requests to the specified target group\.

If you change the protocol from TCP or UDP to TLS, you must specify a security policy and server certificate\. If you change the protocol from TLS to TCP or UDP, the security policy and server certificate are removed\.

If you change the target group for the default action, it has no effect on any active connections to the targets in the original target group\. Connections are not drained or terminated as they would be when deregistering targets\.

**To update your listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Load Balancers**\.

1. Choose the name of the load balancer to open its detail page\.

1. On the **Listeners** tab, choose the text in the **Protocol:Port** column to open the detail page for the listener\.

1. Choose **Edit**\.

1. \(Optional\) Change the specified values for **Protocol** and **Port** as needed\.

1. \(Optional\) Choose a different target group for **Default action**\.

1. \(Optional\) Add, update, or remove tags as needed\.

1. Choose **Save changes**\.

**To update your listener using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command\.