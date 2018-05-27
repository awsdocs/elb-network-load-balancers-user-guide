# Monitor Your Network Load Balancers<a name="load-balancer-monitoring"></a>

You can use the following features to monitor your load balancers, analyze traffic patterns, and troubleshoot issues with your load balancers and targets\.

**CloudWatch metrics**  
You can use Amazon CloudWatch to retrieve statistics about data points for your load balancers and targets as an ordered set of time\-series data, known as *metrics*\. You can use these metrics to verify that your system is performing as expected\. For more information, see [CloudWatch Metrics for Your Network Load Balancer](load-balancer-cloudwatch-metrics.md)\.

**VPC Flow Logs**  
You can use Amazon VPC to capture detailed information about the traffic going to and from your Network Load Balancer\. Create a flow log for each network interface for your load balancer\. There is one network interface per load balancer subnet\. To identify the network interfaces for a Network Load Balancer, look for the name of the load balancer in the description field of the network interface\. For more information, see [VPC Flow Logs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

**CloudTrail logs**  
You can use AWS CloudTrail to capture detailed information about the calls made to the Elastic Load Balancing API and store them as log files in Amazon S3\. You can use these CloudTrail logs to determine which calls were made, the source IP address where the call came from, who made the call, when the call was made, and so on\. For more information, see [AWS CloudTrail Logging for Your Network Load Balancer](load-balancer-cloudtrail-logs.md)\.