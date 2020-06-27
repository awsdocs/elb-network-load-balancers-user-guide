# Update a listener for your Network Load Balancer<a name="listener-update-rules"></a>

You can update the listener port, listener protocol, or the default listener rule\.

The default listener rule forwards requests to the specified target group\.

If you change the protocol from TCP or UDP to TLS, you must specify a security policy and server certificate\. If you change the protocol from TLS to TCP or UDP, the security policy and server certificate are removed\.

**To update your listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Select the check box for the listener and then choose **Edit**\.

1. \(Optional\) Change the specified values for **Protocol : port**\.

1. \(Optional\) Click the pencil icon to select a different target group for **Default action**\.

1. Choose **Update**\.

**To update your listener using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command\.