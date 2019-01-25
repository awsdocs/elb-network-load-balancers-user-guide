# Update Server Certificates<a name="listener-update-certificates"></a>

When you create a TLS listener, you specify a default certificate\.

Each certificate comes with a validity period\. You must ensure that you renew or replace the certificate before its validity period ends\. Renewing or replacing a certificate does not affect in\-flight requests that were received by the load balancer node and are pending routing to a healthy target\. After a certificate is renewed, new requests use the renewed certificate\. After a certificate is replaced, new requests use the new certificate\.

You can manage certificate renewal and replacement as follows:
+ Certificates provided by AWS Certificate Manager and deployed on your load balancer can be renewed automatically\. ACM attempts to renew certificates before they expire\. For more information, see [Managed Renewal](https://docs.aws.amazon.com/acm/latest/userguide/acm-renewal.html) in the *AWS Certificate Manager User Guide*\.
+ If you imported a certificate into ACM, you must monitor the expiration date of the certificate and renew it before it expires\. For more information, see [Importing Certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html) in the *AWS Certificate Manager User Guide*\.
+ If you imported a certificate into IAM and it is about to expire, you must create a new certificate, import the new certificate to ACM or IAM, add the new certificate to your load balancer, and remove the expired certificate from your load balancer\.

**Limitation**  
You cannot install certificates with RSA keys larger than 2048\-bit or EC keys on your Network Load Balancer\.

## Replace the Default Certificate<a name="replace-default-certificate"></a>

You can replace the default certificate for your listener using the following procedure\.

**To change the default certificate using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select the load balancer and choose **Listeners**\.

1. Select the check box for the listener and choose **Edit**\.

1. For **Default SSL certificate**, do one of the following:
   + If you created or imported a certificate using AWS Certificate Manager, choose **From ACM** and choose the certificate\.
   + If you uploaded a certificate using IAM, choose **From IAM** and choose the certificate\.

1. Choose **Update**\.

**To change the default certificate using the AWS CLI**  
Use the [modify\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/modify-listener.html) command\.