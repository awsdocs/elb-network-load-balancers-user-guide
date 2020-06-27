# Monitor your Network Load Balancers<a name="load-balancer-monitoring"></a>

You can use the following features to monitor your load balancers, analyze traffic patterns, and troubleshoot issues with your load balancers and targets\.

**CloudWatch metrics**  
You can use Amazon CloudWatch to retrieve statistics about data points for your load balancers and targets as an ordered set of time\-series data, known as *metrics*\. You can use these metrics to verify that your system is performing as expected\. For more information, see [CloudWatch metrics for your Network Load Balancer](load-balancer-cloudwatch-metrics.md)\.

**VPC Flow Logs**  
You can use VPC Flow Logs to capture detailed information about the traffic going to and from your Network Load Balancer\. For more information, see [VPC flow logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) in the *Amazon VPC User Guide*\.  
Create a flow log for each network interface for your load balancer\. There is one network interface per load balancer subnet\. To identify the network interfaces for a Network Load Balancer, look for the name of the load balancer in the description field of the network interface\.  
There are two entries for each connection through your Network Load Balancer, one for the frontend connection between the client and the load balancer and the other for the backend connection between the load balancer and the target\. If the target is registered by instance ID, the connection appears to the instance as a connection from the client\. If the security group of the instance doesn't allow connections from the client but the network ACLs for the load balancer subnet allow them, the logs for the network interface for the load balancer show "ACCEPT OK" for the frontend and backend connections, while the logs for the network interface for the instance show "REJECT OK" for the connection\.

**Access logs**  
You can use access logs to capture detailed information about TLS requests made to your load balancer\. The log files are stored in Amazon S3\. You can use these access logs to analyze traffic patterns and to troubleshoot issues with your targets\. For more information, see [Access logs for your Network Load Balancer](load-balancer-access-logs.md)\.

**CloudTrail logs**  
You can use AWS CloudTrail to capture detailed information about the calls made to the Elastic Load Balancing API and store them as log files in Amazon S3\. You can use these CloudTrail logs to determine which calls were made, the source IP address where the call came from, who made the call, when the call was made, and so on\. For more information, see [Logging API calls for your Network Load Balancer using AWS CloudTrail](load-balancer-cloudtrail-logs.md)\.