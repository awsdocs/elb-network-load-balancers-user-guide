# Update a Listener for Your Network Load Balancer<a name="listener-update-rules"></a>

You can update the listener port and the listener rule\. Rules determine how your TCP listeners route requests to your target groups\. You initially define the rule for a listener when you create the listener\. For more information, see [Listener Rules](load-balancer-listeners.md#listener-rules)\.

**To update your listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Listener**, select the check box for the listener, and then choose **Actions**, **Edit**\.

1. \(Optional\) Change the specified value for **Port**\.

1. \(Optional\) Select a different target group from **Default target group**\.

1. Choose **Save**\.

**To update your listener using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command\.