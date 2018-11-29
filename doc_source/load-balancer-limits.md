# Limits for Your Network Load Balancers<a name="load-balancer-limits"></a>

To view the current limits for your Network Load Balancers, use the **Limits** page of the Amazon EC2 console, or the [describe\-account\-limits](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-account-limits.html) \(AWS CLI\) command\. To request a limit increase, use the [Elastic Load Balancing Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-elastic-load-balancers)\.

Your AWS account has the following limits related to Network Load Balancers\.

**Regional Limits**
+ Network Load Balancers per region: 20
+ Target groups per region: 3000 **\***

**Load Balancer Limits**
+ Listeners per load balancer: 50
+ Subnets per Availability Zone per load balancer: 1
+ \[Cross\-zone load balancing disabled\] Targets per Availability Zone per load balancer: 500
+ \[Cross\-zone load balancing enabled\] Targets per load balancer: 500
+ Load balancers per target group: 1

**\*** This limit is shared by target groups for your Application Load Balancers and Network Load Balancers\.