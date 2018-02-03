# Tutorial: Create a Network Load Balancer Using the AWS CLI<a name="network-load-balancer-cli"></a>

This tutorial provides a hands\-on introduction to Network Load Balancers through the AWS CLI\.

## Before You Begin<a name="prerequisites-aws-cli"></a>

+ Install the AWS CLI or update to the current version of the AWS CLI if you are using a version that does not support Network Load Balancers\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

+ Decide which Availability Zones you will use for your EC2 instances\. Configure your virtual private cloud \(VPC\) with at least one public subnet in each of these Availability Zones\.

+ Launch at least one EC2 instance in each Availability Zone\. Ensure that the security groups for these instances allow TCP access from clients on the listener port and health check requests from your VPC\. For more information, see [Target Security Groups](target-group-register-targets.md#target-security-groups)\.

## Create Your Load Balancer<a name="create-load-balancer-aws-cli"></a>

To create your first load balancer, complete the following steps\.

**To create a load balancer**

1. Use the [create\-load\-balancer](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command to create a load balancer, specifying a public subnet for each Availability Zone in which you launched instances\. You can specify only one subnet per Availability Zone\.

   ```
   aws elbv2 create-load-balancer --name my-load-balancer --type network --subnets subnet-12345678
   ```

   The output includes the Amazon Resource Name \(ARN\) of the load balancer, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:loadbalancer/net/my-load-balancer/1234567890123456
   ```

1. Use the [create\-target\-group](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create a target group, specifying the same VPC that you used for your EC2 instances:

   ```
   aws elbv2 create-target-group --name my-targets --protocol TCP --port 80 --vpc-id vpc-12345678
   ```

   The output includes the ARN of the target group, with this format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:targetgroup/my-targets/1234567890123456
   ```

1. Use the [register\-targets](http://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to register your instances with your target group:

   ```
   aws elbv2 register-targets --target-group-arn targetgroup-arn --targets Id=i-12345678 Id=i-23456789
   ```

1. Use the [create\-listener](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a listener for your load balancer with a default rule that forwards requests to your target group:

   ```
   aws elbv2 create-listener --load-balancer-arn loadbalancer-arn --protocol TCP --port 80  \
   --default-actions Type=forward,TargetGroupArn=targetgroup-arn
   ```

   The output contains the ARN of the listener, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:listener/net/my-load-balancer/1234567890123456/1234567890123456
   ```

1. \(Optional\) You can verify the health of the registered targets for your target group using this [describe\-target\-health](http://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-target-health.html) command:

   ```
   aws elbv2 describe-target-health --target-group-arn targetgroup-arn
   ```

## Specify an Elastic IP Address for Your Load Balancer<a name="subnet-mappings-aws-cli"></a>

When you create a Network Load Balancer, you can specify one Elastic IP address per subnet using a subnet mapping\.

```
aws elbv2 create-load-balancer --name my-load-balancer --type network \
--subnet-mappings SubnetId=subnet-12345678,AllocationId=eipalloc-12345678
```

## Add Targets Using Port Overrides<a name="port-overrides-aws-cli"></a>

If you have a microservices architecture with multiple services on a single instance, each service accepts connections on a different port\. You can register the instance with the target group multiple times, each time with a different port\.

**To add targets using port overrides**

1. Use the [create\-target\-group](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create a target group:

   ```
   aws elbv2 create-target-group --name my-targets --protocol TCP --port 80 \
   --vpc-id vpc-12345678
   ```

1. Use the [register\-targets](http://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to register your instances with your target group\. Notice that the instance IDs are the same for each container, but the ports are different\.

   ```
   aws elbv2 register-targets --target-group-arn targetgroup-arn  \
   --targets Id=i-12345678,Port=80 Id=i-12345678,Port=766
   ```

1. Use the [create\-listener](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a listener for your load balancer with a default rule that forwards requests to your target group:

   ```
   aws elbv2 create-listener --load-balancer-arn loadbalancer-arn \
   --protocol TCP --port 80  \
   --default-actions Type=forward,TargetGroupArn=targetgroup-arn
   ```

## Delete Your Load Balancer<a name="delete-aws-cli"></a>

When you no longer need your load balancer and target group, you can delete them as follows:

```
aws elbv2 delete-load-balancer --load-balancer-arn loadbalancer-arn
aws elbv2 delete-target-group --target-group-arn targetgroup-arn
```