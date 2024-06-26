[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# GOOGLE / Compute / Autoscale Minimum CPU Utilization Target

## Quick Info

| | |
|-|-|
| **Plugin Title** | Autoscale Minimum CPU Utilization Target |
| **Cloud** | GOOGLE |
| **Category** | Compute |
| **Description** | Ensure that minimum CPU utilization target is greater or equal than set percentage. |
| **More Info** | The autoscaler treats the target CPU utilization level as a fraction of the average use of all vCPUs over time in the instance group. If the average utilization of your total vCPUs exceeds the target utilization, the autoscaler adds more VM instances. If the average utilization of your total vCPUs is less than the target utilization, the autoscaler removes instances. |
| **GOOGLE Link** | https://cloud.google.com/compute/docs/autoscaler/scaling-cpu |
| **Recommended Action** | Ensure all instance groups have Minimum CPU Utilization greater than or equal to target value. |

## Detailed Remediation Steps
1.  In the console, go to the Instance groups page.

    [Go to Instance groups](https://console.cloud.google.com/compute/instanceGroups)

2.  If you have an instance group, select it and click Edit. If you don't have an instance group, click Create instance group.

3.  If no autoscaling configuration exists, under Autoscaling, click Configure autoscaling.

4.  Under Autoscaling mode, select On: add and remove instances to the group to enable autoscaling.

5.  Specify the minimum and maximum numbers of instances that you want the autoscaler to create in this group.

6.  In the Autoscaling metrics section, if an existing CPU utilization metric does not yet exist, add one:

    a.  Click Add metric.
    b.  Under Metric type, select CPU utilization.
    c.  Enter the Target CPU utilization that you want. This value is treated as a percentage. For example, for 75% CPU utilization, enter `75`.
    d.  Under Predictive autoscaling, select Off. To learn more about predictive autoscaling, and whether it is suitable for your workload, see [Scaling based on predictions](https://cloud.google.com/compute/docs/autoscaler/predictive-autoscaling).
    e.  Click Done.
7.  You can use the Cool down period to tell the autoscaler how long it takes for your application to initialize. Specifying an accurate cool down period improves autoscaler decisions. For example, when scaling out, the autoscaler ignores data from VMs that are still initializing because those VMs might not yet represent normal usage of your application. The default cool down period is 60 seconds.

8.  Click Save.