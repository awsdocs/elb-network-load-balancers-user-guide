# Tutorial: Create a Network Load Balancer using the AWS CLI<a name="network-load-balancer-cli"></a>

This tutorial provides a hands\-on introduction to Network Load Balancers through the AWS CLI\.

## Before you begin<a name="prerequisites-aws-cli"></a>
+ Install the AWS CLI or update to the current version of the AWS CLI if you are using a version that does not support Network Load Balancers\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.
+ Decide which Availability Zones you will use for your EC2 instances\. Configure your virtual private cloud \(VPC\) with at least one public subnet in each of these Availability Zones\.
+ Decide if you will create an IPv4 or dualstack load balancer\. Use IPv4 if you want clients to communicate with the load balancer using IPv4 addresses only\. Use dualstack if you want clients to communicate with the load balancer using IPv4 and IPv6 addresses\. You can also use dualstack to communicate with backend targets, such as IPv6 applications or dualstack subnets, using IPv6\. 
+ Launch at least one EC2 instance in each Availability Zone\. Ensure that the security groups for these instances allow TCP access from clients on the listener port and health check requests from your VPC\. For more information, see [Target security groups](target-group-register-targets.md#target-security-groups)\.

## Create your IPv4 load balancer<a name="create-ipv4-load-balancer-aws-cli"></a>

To create your first load balancer, complete the following steps\.

**To create a IPv4 load balancer**

1. Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command to create an IPv4 load balancer, specifying a public subnet for each Availability Zone in which you launched instances\. You can specify only one subnet per Availability Zone\.

   ```
   aws elbv2 create-load-balancer --name my-load-balancer --type network --subnets subnet-0e3f5cac72EXAMPLE
   ```

   The output includes the Amazon Resource Name \(ARN\) of the load balancer, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:loadbalancer/net/my-load-balancer/1234567890123456
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create an IPv4 target group, specifying the same VPC that you used for your EC2 instances\. IPv4 target groups support IP and instance type targets\.

   ```
   aws elbv2 create-target-group --name my-targets --protocol TCP --port 80 --vpc-id vpc-0598c7d356EXAMPLE
   ```

   The output includes the ARN of the target group, with this format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:targetgroup/my-targets/1234567890123456
   ```

1. Use the [register\-targets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to register your instances with your target group:

   ```
   aws elbv2 register-targets --target-group-arn targetgroup-arn --targets Id=i-1234567890abcdef0 Id=i-0abcdef1234567890
   ```

1. Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a listener for your load balancer with a default rule that forwards requests to your target group:

   ```
   aws elbv2 create-listener --load-balancer-arn loadbalancer-arn --protocol TCP --port 80  \
   --default-actions Type=forward,TargetGroupArn=targetgroup-arn
   ```

   The output contains the ARN of the listener, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:listener/net/my-load-balancer/1234567890123456/1234567890123456
   ```

1. \(Optional\) You can verify the health of the registered targets for your target group using this [describe\-target\-health](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-target-health.html) command:

   ```
   aws elbv2 describe-target-health --target-group-arn targetgroup-arn
   ```

## Create your dualstack load balancer<a name="create-dualstack-load-balancer-aws-cli"></a>

To create your first load balancer, complete the following steps\.

**To create a dualstack load balancer**

1. Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command to create a dualstack load balancer, specifying a public subnet for each Availability Zone in which you launched instances\. You can specify only one subnet per Availability Zone\.

   ```
   aws elbv2 create-load-balancer --name my-load-balancer --type network --subnets subnet-0e3f5cac72EXAMPLE --ip-address-type dualstack
   ```

   The output includes the Amazon Resource Name \(ARN\) of the load balancer, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:loadbalancer/net/my-load-balancer/1234567890123456
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create a target group, specifying the same VPC that you used for your EC2 instances\. 

   You must use either a TCP or TLS target group with your dualstack load balancer\.

   You can create IPv4 and IPv6 target groups to associate with dualstack load balancers\. The target group's IP address type determines the IP version that the load balancer will use to both communicate with, and check the health of, your backend targets\. 

   IPv4 target groups support IP and instance type targets\. IPv6 targets only support IP targets\.

   ```
   aws elbv2 create-target-group --name my-targets --protocol TCP --port 80 --vpc-id vpc-0598c7d356EXAMPLE  --ip-address-type [ipv4 or ipv6]
   ```

   The output includes the ARN of the target group, with this format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:targetgroup/my-targets/1234567890123456
   ```

1. Use the [register\-targets](https://docs.aws.amazon.com/cli/latest/reference/elbv2/register-targets.html) command to register your instances with your target group:

   ```
   aws elbv2 register-targets --target-group-arn targetgroup-arn --targets Id=i-1234567890abcdef0 Id=i-0abcdef1234567890
   ```

1. Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a listener for your load balancer with a default rule that forwards requests to your target group\. Dualstack load balancers must have TCP or TLS listeners\.

   ```
   aws elbv2 create-listener --load-balancer-arn loadbalancer-arn --protocol TCP --port 80  \
   --default-actions Type=forward,TargetGroupArn=targetgroup-arn
   ```

   The output contains the ARN of the listener, with the following format:

   ```
   arn:aws:elasticloadbalancing:us-east-2:123456789012:listener/net/my-load-balancer/1234567890123456/1234567890123456
   ```

1. \(Optional\) You can verify the health of the registered targets for your target group using this [describe\-target\-health](https://docs.aws.amazon.com/cli/latest/reference/elbv2/describe-target-health.html) command:

   ```
   aws elbv2 describe-target-health --target-group-arn targetgroup-arn
   ```

## Specify an Elastic IP address for your load balancer<a name="subnet-mappings-aws-cli"></a>

When you create a Network Load Balancer, you can specify one Elastic IP address per subnet using a subnet mapping\.

```
aws elbv2 create-load-balancer --name my-load-balancer --type network \
--subnet-mappings SubnetId=subnet-0e3f5cac72EXAMPLE,AllocationId=eipalloc-12345678
```

## Delete your load balancer<a name="delete-aws-cli"></a>

When you no longer need your load balancer and target group, you can delete them as follows:

```
aws elbv2 delete-load-balancer --load-balancer-arn loadbalancer-arn
aws elbv2 delete-target-group --target-group-arn targetgroup-arn
```