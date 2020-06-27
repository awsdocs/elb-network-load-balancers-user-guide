# Create a listener for your Network Load Balancer<a name="create-listener"></a>

A listener is a process that checks for connection requests\. You define a listener when you create your load balancer, and you can add listeners to your load balancer at any time\.

## Prerequisites<a name="listener-prereqs"></a>
+ You must specify a target group for the listener rule\. For more information, see [Create a target group for your Network Load Balancer](create-target-group.md)\.
+ You must specify an SSL certificate for a TLS listener\. The load balancer uses the certificate to terminate the connection and decrypt requests from clients before routing them to targets\. For more information, see [Server certificates](create-tls-listener.md#tls-listener-certificates)\.

## Add a listener<a name="add-listener"></a>

You configure a listener with a protocol and a port for connections from clients to the load balancer, and a target group for the default listener rule\. For more information, see [Listener configuration](load-balancer-listeners.md#listener-configuration)\.

**To add a listener using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Choose **Add listener**\.

1. For **Protocol : port**, choose **TCP**, **UDP**, **TCP\_UDP**, or **TLS**\. Keep the default port or type a different port\.

1. \[TLS listeners\] For **ALPN policy**, choose a policy to enable ALPN or choose **None** to disable ALPN\. For more information, see [ALPN policies](create-tls-listener.md#alpn-policies)\.

1. For **Default actions**, choose **Add action**, **Forward to** and then choose an available target group\.

1. \[TLS listeners\] For **Security policy**, we recommend that you keep the default security policy\.

1. \[TLS listeners\] For **Default SSL certificate**, do one of the following:
   + If you created or imported a certificate using AWS Certificate Manager, choose **From ACM** and choose the certificate\.
   + If you uploaded a certificate using IAM, choose **From IAM** and choose the certificate\.

1. Choose **Save**\.

1. \[TLS listeners\] To add an optional certificate list for use with the SNI protocol, see [Add certificates to the certificate list](listener-update-certificates.md#add-certificates)\.

**To add a listener using the AWS CLI**  
Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create the listener\.