# Quotas for your Network Load Balancers<a name="load-balancer-limits"></a>

Your AWS account has default quotas, formerly referred to as limits, for each AWS service\. Unless otherwise noted, each quota is Region\-specific\. You can request increases for some quotas, and other quotas cannot be increased\.

To view the quotas for your Network Load Balancers, open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\. In the navigation pane, choose **AWS services** and select **Elastic Load Balancing**\. You can also use the [describe\-account\-limits](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-account-limits.html) \(AWS CLI\) command for Elastic Load Balancing\.

To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota is not yet available in Service Quotas, use the [Elastic Load Balancing limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-elastic-load-balancers)\.

**Load balancer**  
Your AWS account has the following quotas related to Network Load Balancers\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Certificates per Network Load Balancer |  25  | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-52964454) | 
| Listeners per Network Load Balancer |  50  | No | 
| Network Load Balancer ENIs per VPC  |  1,200 \* | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-23568085) | 
| Network Load Balancers per Region |  50  | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-69A177A2) | 
| Target Groups per Action per Network Load Balancer |  1  | No | 
| Targets per Availability Zone per Network Load Balancer  |  500 † | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-B211E961) | 
| Targets per Network Load Balancer |  3,000  | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-EEF1AD04) | 

**\*** Each Network Load Balancer uses one network interface per zone\. The quota is set at the VPC level\. When sharing subnets or VPCs, the usage is calculated across all tenants\.

† If a target is registered with *N* target groups, it counts as *N* targets toward this limit\. Each Application Load Balancer that is a target of the Network Load Balancer counts as 50 targets if cross\-zone load balancing is disabled or 100 targets if cross\-zone load balancing is enabled\.

**Target groups**  
The following quotas are for target groups\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Target Groups per Region |  3,000 \*\* | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-B22855CB) | 
| Targets per Target Group per Region \(instances or IP addresses\) |  1,000  | [Yes](https://console.aws.amazon.com/servicequotas/home/services/elasticloadbalancing/quotas/L-A0D0B863) | 
| Targets per Target Group per Region \(Application Load Balancers\) | 1 | No | 

**\*\*** This quota is shared by Application Load Balancers and Network Load Balancers\.