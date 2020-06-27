# Delete a Network Load Balancer<a name="load-balancer-delete"></a>

As soon as your load balancer becomes available, you are billed for each hour or partial hour that you keep it running\. When you no longer need the load balancer, you can delete it\. As soon as the load balancer is deleted, you stop incurring charges for it\.

You can't delete a load balancer if deletion protection is enabled\. For more information, see [Deletion protection](network-load-balancers.md#deletion-protection)\.

You can't delete a load balancer if it is in use by another service\. For example, if the load balancer is associated with a VPC endpoint service, you must delete the endpoint service configuration before you can delete the associated load balancer\.

Deleting a load balancer also deletes its listeners\. Deleting a load balancer does not affect its registered targets\. For example, your EC2 instances continue to run and are still registered to their target groups\. To delete your target groups, see [Delete a target group](delete-target-group.md)\.

**To delete a load balancer using the console**

1. If you have a CNAME record for your domain that points to your load balancer, point it to a new location and wait for the DNS change to take effect before deleting your load balancer\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.

**To delete a load balancer using the AWS CLI**  
Use the [delete\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/delete-load-balancer.html) command\.