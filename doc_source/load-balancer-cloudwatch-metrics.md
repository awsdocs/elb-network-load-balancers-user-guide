# CloudWatch metrics for your Network Load Balancer<a name="load-balancer-cloudwatch-metrics"></a>

Elastic Load Balancing publishes data points to Amazon CloudWatch for your load balancers and your targets\. CloudWatch enables you to retrieve statistics about those data points as an ordered set of time\-series data, known as *metrics*\. Think of a metric as a variable to monitor, and the data points as the values of that variable over time\. For example, you can monitor the total number of healthy targets for a load balancer over a specified time period\. Each data point has an associated time stamp and an optional unit of measurement\.

You can use metrics to verify that your system is performing as expected\. For example, you can create a CloudWatch alarm to monitor a specified metric and initiate an action \(such as sending a notification to an email address\) if the metric goes outside what you consider an acceptable range\.

Elastic Load Balancing reports metrics to CloudWatch only when requests are flowing through the load balancer\. If there are requests flowing through the load balancer, Elastic Load Balancing measures and sends its metrics in 60\-second intervals\. If there are no requests flowing through the load balancer or no data for a metric, the metric is not reported\.

For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Topics**
+ [Network Load Balancer metrics](#load-balancer-metrics-nlb)
+ [Metric dimensions for Network Load Balancers](#load-balancer-metric-dimensions-nlb)
+ [Statistics for Network Load Balancer metrics](#metric-statistics)
+ [View CloudWatch metrics for your load balancer](#view-metric-data)

## Network Load Balancer metrics<a name="load-balancer-metrics-nlb"></a>

The `AWS/NetworkELB` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| ActiveFlowCount |  The total number of concurrent flows \(or connections\) from clients to targets\. This metric includes connections in the SYN\_SENT and ESTABLISHED states\. TCP connections are not terminated at the load balancer, so a client opening a TCP connection to a target counts as a single flow\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistics are `Average`, `Maximum`, and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ActiveFlowCount\_TCP |  The total number of concurrent TCP flows \(or connections\) from clients to targets\. This metric includes only connections in the ESTABLISHED state\. TCP connections are not terminated at the load balancer, so a client opening a TCP connection to a target counts as a single flow\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistics are `Average`, `Maximum`, and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ActiveFlowCount\_TLS |  The total number of concurrent TLS flows \(or connections\) from clients to targets\. This metric includes only connections in the ESTABLISHED state\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistics are `Average`, `Maximum`, and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ActiveFlowCount\_UDP |  The total number of concurrent UDP flows \(or connections\) from clients to targets\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistics are `Average`, `Maximum`, and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ClientTLSNegotiationErrorCount |  The total number of TLS handshakes that failed during negotiation between a client and a TLS listener\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ConsumedLCUs |  The number of load balancer capacity units \(LCU\) used by your load balancer\. You pay for the number of LCUs that you use per hour\. For more information, see [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. **Reporting criteria**: Always reported **Statistics**: All [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ConsumedLCUs\_TCP |  The number of load balancer capacity units \(LCU\) used by your load balancer for TCP\. You pay for the number of LCUs that you use per hour\. For more information, see [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. **Reporting criteria**: Always reported **Statistics**: All [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ConsumedLCUs\_TLS |  The number of load balancer capacity units \(LCU\) used by your load balancer for TLS\. You pay for the number of LCUs that you use per hour\. For more information, see [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. **Reporting criteria**: Always reported **Statistics**: All [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ConsumedLCUs\_UDP |  The number of load balancer capacity units \(LCU\) used by your load balancer for UDP\. You pay for the number of LCUs that you use per hour\. For more information, see [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. **Reporting criteria**: Always reported **Statistics**: All [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| HealthyHostCount |  The number of targets that are considered healthy\. **Reporting criteria**: Reported if health checks are enabled **Statistics**: The most useful statistics are `Maximum` and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| NewFlowCount |  The total number of new flows \(or connections\) established from clients to targets in the time period\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| NewFlowCount\_TCP |  The total number of new TCP flows \(or connections\) established from clients to targets in the time period\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| NewFlowCount\_TLS |  The total number of new TLS flows \(or connections\) established from clients to targets in the time period\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| NewFlowCount\_UDP |  The total number of new UDP flows \(or connections\) established from clients to targets in the time period\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ProcessedBytes |  The total number of bytes processed by the load balancer, including TCP/IP headers\. This count includes traffic to and from targets, minus health check traffic\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ProcessedBytes\_TCP |  The total number of bytes processed by TCP listeners\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ProcessedBytes\_TLS |  The total number of bytes processed by TLS listeners\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| ProcessedBytes\_UDP |  The total number of bytes processed by UDP listeners\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| TargetTLSNegotiationErrorCount |  The total number of TLS handshakes that failed during negotiation between a TLS listener and a target\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| TCP\_Client\_Reset\_Count |  The total number of reset \(RST\) packets sent from a client to a target\. These resets are generated by the client and forwarded by the load balancer\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| TCP\_ELB\_Reset\_Count |  The total number of reset \(RST\) packets generated by the load balancer\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| TCP\_Target\_Reset\_Count |  The total number of reset \(RST\) packets sent from a target to a client\. These resets are generated by the target and forwarded by the load balancer\. **Reporting criteria**: There is a nonzero value **Statistics**: The most useful statistic is `Sum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 
| UnHealthyHostCount |  The number of targets that are considered unhealthy\. **Reporting criteria**: Reported if health checks are enabled **Statistics**: The most useful statistics are `Maximum` and `Minimum`\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html)  | 

## Metric dimensions for Network Load Balancers<a name="load-balancer-metric-dimensions-nlb"></a>

To filter the metrics for your load balancer, use the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| AvailabilityZone |  Filters the metric data by Availability Zone\.  | 
| LoadBalancer |  Filters the metric data by load balancer\. Specify the load balancer as follows: net/*load\-balancer\-name*/*1234567890123456* \(the final portion of the load balancer ARN\)\.  | 
| TargetGroup |  Filters the metric data by target group\. Specify the target group as follows: targetgroup/*target\-group\-name*/*1234567890123456* \(the final portion of the target group ARN\)\.  | 

## Statistics for Network Load Balancer metrics<a name="metric-statistics"></a>

CloudWatch provides statistics based on the metric data points published by Elastic Load Balancing\. Statistics are metric data aggregations over specified period of time\. When you request statistics, the returned data stream is identified by the metric name and dimension\. A dimension is a name/value pair that uniquely identifies a metric\. For example, you can request statistics for all the healthy EC2 instances behind a load balancer launched in a specific Availability Zone\.

The `Minimum` and `Maximum` statistics reflect the minimum and maximum values of the data points reported by the individual load balancer nodes in each sampling window\. Increases in the maximum of `HealthyHostCount` correspond to decreases in the minimum of `UnHealthyHostCount`\. Therefore, we recommend that you monitor your Network Load Balancer using either the maximum of `HealthyHostCount` or the minimum of `UnHealthyHostCount`\.

The `Sum` statistic is the aggregate value across all load balancer nodes\. Because metrics include multiple reports per period, `Sum` is only applicable to metrics that are aggregated across all load balancer nodes\.

The `SampleCount` statistic is the number of samples measured\. Because metrics are gathered based on sampling intervals and events, this statistic is typically not useful\. For example, with `HealthyHostCount`, `SampleCount` is based on the number of samples that each load balancer node reports, not the number of healthy hosts\.

## View CloudWatch metrics for your load balancer<a name="view-metric-data"></a>

You can view the CloudWatch metrics for your load balancers using the Amazon EC2 console\. These metrics are displayed as monitoring graphs\. The monitoring graphs show data points if the load balancer is active and receiving requests\.

Alternatively, you can view metrics for your load balancer using the CloudWatch console\.

**To view metrics using the Amazon EC2 console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. To view metrics filtered by target group, do the following:

   1. In the navigation pane, choose **Target Groups**\.

   1. Select your target group and choose **Monitoring**\.

   1. \(Optional\) To filter the results by time, select a time range from **Showing data for**\.

   1. To get a larger view of a single metric, select its graph\.

1. To view metrics filtered by load balancer, do the following:

   1. In the navigation pane, choose **Load Balancers**\.

   1. Select your load balancer and choose **Monitoring**\.

   1. \(Optional\) To filter the results by time, select a time range from **Showing data for**\.

   1. To get a larger view of a single metric, select its graph\.

**To view metrics using the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select the **NetworkELB** namespace\.

1. \(Optional\) To view a metric across all dimensions, type its name in the search field\.

**To view metrics using the AWS CLI**  
Use the following [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command to list the available metrics:

```
aws cloudwatch list-metrics --namespace AWS/NetworkELB
```

**To get the statistics for a metric using the AWS CLI**  
Use the following [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) command get statistics for the specified metric and dimension\. Note that CloudWatch treats each unique combination of dimensions as a separate metric\. You can't retrieve statistics using combinations of dimensions that were not specially published\. You must specify the same dimensions that were used when the metrics were created\.

```
aws cloudwatch get-metric-statistics --namespace AWS/NetworkELB \
--metric-name UnHealthyHostCount --statistics Average  --period 3600 \
--dimensions Name=LoadBalancer,Value=net/my-load-balancer/50dc6c495c0c9188 \
Name=TargetGroup,Value=targetgroup/my-targets/73e2d6bc24d8a067 \
--start-time 2017-04-18T00:00:00Z --end-time 2017-04-21T00:00:00Z
```

The following is example output:

```
{
    "Datapoints": [
        {
            "Timestamp": "2017-04-18T22:00:00Z",
            "Average": 0.0,
            "Unit": "Count"
        },
        {
            "Timestamp": "2017-04-18T04:00:00Z",
            "Average": 0.0,
            "Unit": "Count"
        },
        ...
    ],
    "Label": "UnHealthyHostCount"
}
```