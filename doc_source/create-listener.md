# Create a Listener for Your Network Load Balancer<a name="create-listener"></a>

A listener is a process that checks for connection requests\. You define a listener when you create your load balancer, and you can add listeners to your load balancer at any time\.

## Prerequisites<a name="listener-prereqs"></a>

+ You must specify a target group for the listener rule\. For more information, see [Create a Target Group for Your Network Load Balancer](create-target-group.md)\.

## Add a Listener<a name="add-listener"></a>

You configure a listener with a protocol and a port for connections from clients to the load balancer, and a target group for the default listener rule\. For more information, see [Listener Configuration](load-balancer-listeners.md#listener-configuration)\.

**To add a listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer\.

1. Choose **Listener**, **Add listener**\.

1. For **Protocol**, keep **TCP**\.

1. For **Port**, type the listener port\.

1. For **Default target group**, select an available target group with the TCP protocol\.

1. Choose **Create**\.

**To add a listener using the AWS CLI**  
Use the [create\-listener](http://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create the listener\.