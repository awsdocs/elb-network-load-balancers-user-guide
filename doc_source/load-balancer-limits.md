# Quotas for your Network Load Balancers<a name="load-balancer-limits"></a>

Your AWS account has default quotas, formerly referred to as limits, for each AWS service\. Unless otherwise noted, each quota is Region\-specific\. You can request increases for some quotas, and other quotas cannot be increased\.

To view the quotas for your Network Load Balancers, open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\. In the navigation pane, choose **AWS services** and select **Elastic Load Balancing**\. You can also use the [describe\-account\-limits](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-account-limits.html) \(AWS CLI\) command for Elastic Load Balancing\.

To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota is not yet available in Service Quotas, use the [Elastic Load Balancing limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-elastic-load-balancers)\.

Your AWS account has the following quotas related to Network Load Balancers\.

**Regional**
+ Network Load Balancers per region: 50
+ Target groups per region: 3000 **\***

**Load balancer**
+ Listeners per load balancer: 50
+ Subnets per Availability Zone per load balancer: 1
+ \[Cross\-zone load balancing disabled\] Targets per Availability Zone per load balancer: 500
+ \[Cross\-zone load balancing enabled\] Targets per load balancer: 500
+ Load balancers per target group: 1
+ Certificates per load balancer \(not counting default certificates\): 25

**\*** This quota is shared by target groups for your Application Load Balancers and Network Load Balancers\.