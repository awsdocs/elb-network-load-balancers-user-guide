# Update a TLS listener for your Network Load Balancer<a name="listener-update-certificates"></a>

After you create a TLS listener, you can replace the default certificate, add or remove certificates from the certificate list, update the security policy, or update the ALPN policy\.

**Limitation**  
You cannot install certificates with RSA keys larger than 2048\-bit or EC keys on your Network Load Balancer\.

**Topics**
+ [Replace the default certificate](#replace-default-certificate)
+ [Add certificates to the certificate list](#add-certificates)
+ [Remove certificates from the certificate list](#remove-certificates)
+ [Update the security policy](#update-security-policy)
+ [Update the ALPN policy](#update-alpn-policy)

## Replace the default certificate<a name="replace-default-certificate"></a>

You can replace the default certificate for your TLS listener using the following procedure\. For more information, see [Default certificate](create-tls-listener.md#default-certificate)\.

**To replace the default certificate using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Select the check box for the listener and choose **Edit**\.

1. For **Default SSL certificate**, do one of the following:
   + If you created or imported a certificate using AWS Certificate Manager, choose **From ACM** and choose the certificate\.
   + If you uploaded a certificate using IAM, choose **From IAM** and choose the certificate\.

1. Choose **Update**\.

**To replace the default certificate using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command with the \-\-certificates option\.

## Add certificates to the certificate list<a name="add-certificates"></a>

You can add certificates to the certificate list for your listener using the following procedure\. When you first create a TLS listener, the certificate list is empty\. You can add one or more certificates\. You can optionally add the default certificate to ensure that this certificate is used with the SNI protocol even if it is replaced as the default certificate\. For more information, see [Certificate list](create-tls-listener.md#sni-certificate-list)\.

**To add certificates to the certificate list using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. For the HTTPS listener to update, choose **View/edit certificates**, which displays the default certificate followed by any other certificates that you've added to the listener\.

1. Choose the **Add certificates** icon \(the plus sign\) in the menu bar, which displays the default certificate followed by any other certificates managed by ACM and IAM\. If you've already added a certificate to the listener, its check box is selected and disabled\.

1. To add certificates that are already managed by ACM or IAM, select the check boxes for the certificates and choose **Add**\.

1. If you have a certificate that isn't managed by ACM or IAM, import it to ACM and add it to your listener as follows:

   1. Choose **Import certificate**\.

   1. For **Certificate private key**, paste the PEM\-encoded, unencrypted private key for the certificate\.

   1. For **Certificate body**, paste the PEM\-encoded certificate\.

   1. \(Optional\) For **Certificate chain**, paste the PEM\-encoded certificate chain\.

   1. Choose **Import**\. The newly imported certificate appears in the list of available certificates and is selected\.

   1. Choose **Add**\.

1. To leave this screen, choose the **Back to the load balancer** icon \(the back button\) in the menu bar\.

**To add a certificate to the certificate list using the AWS CLI**  
Use the [add\-listener\-certificates](https://docs.aws.amazon.com/cli/latest/reference/elbv2/add-listener-certificates.html) command\.

## Remove certificates from the certificate list<a name="remove-certificates"></a>

You can remove certificates from the certificate list for a TLS listener using the following procedure\. To remove the default certificate for a TLS listener, see [Replace the default certificate](#replace-default-certificate)\.

**To remove certificates from the certificate list using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. For the listener to update, choose **View/edit certificates**, which displays the default certificate followed by any other certificates that you've added to the listener\.

1. Choose the **Remove certificates** icon \(the minus sign\) in the menu bar\.

1. Select the check boxes for the certificates and choose **Remove**\.

1. To leave this screen, choose the **Back to the load balancer** icon \(the back button\) in the menu bar\.

**To remove a certificate from the certificate list using the AWS CLI**  
Use the [remove\-listener\-certificates](https://docs.aws.amazon.com/cli/latest/reference/elbv2/remove-listener-certificates.html) command\.

## Update the security policy<a name="update-security-policy"></a>

When you create a TLS listener, you can select the security policy that meets your needs\. When a new security policy is added, you can update your TLS listener to use the new security policy\. Network Load Balancers do not support custom security policies\. For more information, see [Security policies](create-tls-listener.md#describe-ssl-policies)\.

**To update the security policy using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Select the check box for the TLS listener and choose **Edit**\.

1. For **Security policy**, choose a security policy\.

1. Choose **Update**\.

**To update the security policy using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command with the \-\-ssl\-policy option\.

## Update the ALPN policy<a name="update-alpn-policy"></a>

You can update the ALPN policy for your TLS listener using the following procedure\. For more information, see [ALPN policies](create-tls-listener.md#alpn-policies)\.

**To update the ALPN policy using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Select the check box for the TLS listener and choose **Edit**\.

1. For **ALPN policy**, choose a policy to enable ALPN or choose **None** to disable ALPN\.

1. Choose **Update**\.

**To update the ALPN policy using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command with the \-\-alpn\-policy option\.