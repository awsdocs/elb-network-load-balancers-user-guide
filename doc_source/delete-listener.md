# Delete a Listener for Your Network Load Balancer<a name="delete-listener"></a>

You can delete a listener at any time\. When you delete a load balancer, its listeners are deleted\.

**To delete a listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers** and select the load balancer\.

1. Choose **Listener**, select the check box for the listener, and then choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.

**To delete a listener using the AWS CLI**  
Use the [delete\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/delete-listener.html) command\.