# Cross\-zone load balancing for target groups<a name="target-group-cross-zone"></a>

The nodes for your load balancer distribute requests from clients to registered targets\. When cross\-zone load balancing is on, each load balancer node distributes traffic across the registered targets in all registered Availability Zones\. When cross\-zone load balancing is off, each load balancer node distributes traffic across only the registered targets in its Availability Zone\. This could be used if zonal failure domains are preferred over regional, ensuring that a healthy zone isn't impacted by an unhealthy zone, or for overall latency improvements\.

With Network Load Balancers, cross\-zone load balancing is off by default at the load balancer level, but you can turn it on at any time\. For target groups, the default is to use the load balancer setting, but you can override the default by explicitly turning cross\-zone load balancing on or off at the target group level\.

**Considerations**
+ The target group setting determines the load balancing behavior for the target group\. For example, if cross\-zone load balancing is enabled at the load balancer level and disabled at the target group level, traffic sent to the target group is not routed across Availability Zones\.
+ When cross\-zone load balancing is off, ensure that you have enough target capacity in each of the load balancer Availability Zones, so that each zone can serve its associated workload\.
+ When cross\-zone load balancing is off, ensure that all target groups participate in the same Availability Zones\. An empty Availability Zone is considered unhealthy\.

## Modify cross\-zone load balancing for a load balancer<a name="modify-load-balancer-cross-zone"></a>

You can turn cross\-zone load balancing on or off at the load balancer level at any time\.

**To modify cross\-zone load balancing for a load balancer using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the name of the load balancer to open its details page\.

1. On the **Attributes** tab, choose **Edit**\.

1. On the **Edit load balancer attributes** page, turn **Cross\-zone load balancing** on or off\.

1. Choose **Save changes**\.

**To modify cross\-zone load balancing for your load balancer using the AWS CLI**  
Use the [modify\-load\-balancer\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-load-balancer-attributes.html) command with the `load_balancing.cross_zone.enabled` attribute\.

## Modify cross\-zone load balancing for a target group<a name="modify-target-group-cross-zone"></a>

The cross\-zone load balancing setting at the target group level overrides the setting at the load balancer level\.

You can turn cross\-zone load balancing on or off at the target group level if the target group type is `instance` or `ip`\. If the target group type is `alb`, the target group always inherits the cross\-zone load balancing setting from the load balancer\.

**To modify cross\-zone load balancing for a target group using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Load Balancing**, select **Target Groups**\.

1. Select the name of the target group to open its details page\.

1. On the **Attributes** tab, choose **Edit**\.

1. On the **Edit target group attributes** page, select **On** for **Cross\-zone load balancing**\.

1. Choose **Save changes**\.

**To modify cross\-zone load balancing for a target group using the AWS CLI**  
Use the [modify\-target\-group\-attributes](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-target-group-attributes.html) command with the `load_balancing.cross_zone.enabled` attribute\.