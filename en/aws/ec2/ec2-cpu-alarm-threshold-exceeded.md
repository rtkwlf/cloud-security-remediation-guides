# AWS / EC2 / EC2 CPU Alarm Threshold Exceeded

## Quick Info

| | |
|-|-|
| **Plugin Title** | EC2 CPU Alarm Threshold Exceeded |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensure EC2 instances do not exceed the alarm threshold for CPU utilization. |
| **More Info** |Excessive CPU utilization can indicate performance issues or the need for capacity optimization.' |
| **AWS Link** | https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html#ec2-cloudwatch-metrics |
| **Recommended Action** | Investigate the cause of high CPU utilization and consider optimizing or scaling resources. |

### Introduction

This guide provides detailed steps to investigate, optimize, and scale Amazon EC2 instances experiencing high CPU utilization. Excessive CPU utilization indicates potential performance bottlenecks, degraded application responsiveness, and increased operational costs. Addressing this issue ensures your EC2 instances maintain optimal performance and resource efficiency.

### Prerequisites

Before proceeding with the remediation steps, ensure the following prerequisites are met:

1.  **AWS Systems Manager (SSM) Agent Installed**: The SSM Agent must be installed and running on your EC2 instances. This agent is pre-installed on some AWS AMIs, but for others, manual installation may be required.
    *   [Manually installing and uninstalling SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/manually-install-ssm-agent-linux.html)
    *   [Manually installing and uninstalling SSM Agent on EC2 instances for Windows Server](https://docs.aws.amazon.com/systems-manager/latest/userguide/manually-install-ssm-agent-windows.html)
2.  **CloudWatch Agent Installed**: The CloudWatch Agent should be installed on your EC2 instances to collect detailed system-level metrics, including memory, disk, and more granular CPU metrics beyond the default EC2 metrics.
    *   [Collect metrics, logs, and traces using the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)
3.  **IAM Permissions**: Ensure the EC2 instance's IAM role has the necessary permissions to publish metrics to CloudWatch. This is typically handled by the `CloudWatchAgentServerPolicy` managed policy.
4.  **Access to AWS Management Console or AWS CLI**: You will need appropriate permissions to access EC2, CloudWatch, and Auto Scaling services.

### Detailed Remediation Steps

Follow these steps to investigate the cause of high CPU utilization and implement appropriate optimizations or scaling solutions.

1.  **Investigate the Cause of High CPU Utilization**
    *   **Monitor with CloudWatch**:
        *   Navigate to the CloudWatch console.
        *   Go to "Metrics" -> "All metrics" -> "EC2" -> "Per-Instance Metrics".
        *   Select the `CPUUtilization` metric for the affected instance(s).
        *   Analyze the graph to identify patterns, peak times, and the duration of high CPU usage.
        *   If the CloudWatch Agent is installed, also review custom metrics under the `CWAgent` namespace for more granular CPU usage (e.g., `cpu_usage_active`, `cpu_usage_idle`, `cpu_usage_system`, `cpu_usage_user`) to understand the type of workload consuming CPU.
    *   **Access the Instance and Identify Processes**:
        *   **For Linux/Unix instances**: Connect to the EC2 instance using SSH. Run the `top` command (or `htop` if installed) to view real-time CPU usage by processes. Identify the processes consuming the most CPU.
        *   **For Windows Server instances**: Connect to the EC2 instance using RDP. Open Task Manager and navigate to the "Processes" or "Details" tab. Sort by CPU usage to identify the processes consuming the most CPU.
        *   Analyze the identified processes to determine if they are expected application workloads, background tasks, or potentially rogue processes.

2.  **Optimize Instance Performance**
    *   **Disable Unnecessary Services/Software**:
        *   Based on your investigation in Step 1, identify any services or software running on the instance that are not critical to your application's workload.
        *   Disable or uninstall these unnecessary components to free up CPU resources.
    *   **Optimize Application Code/Configuration**:
        *   If your application's processes are the primary cause of high CPU, review the application's code, configuration, and logs for inefficiencies.
        *   Consider profiling the application to pinpoint CPU-intensive functions or operations.
    *   **Customize CPU Options (Advanced)**:
        *   For specific workloads (e.g., licensing cost optimization, HPC), you can customize the number of CPU cores and threads per core.
        *   **Note**: This is an advanced option and should be considered carefully. It's not supported for all instance types (e.g., T2, C7a, M7a, R7a, Apple silicon Mac instances, and AWS Graviton processor-based instances).
        *   You can specify CPU options during or after instance launch. Refer to [CPU options for Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-optimize-cpu.html) for detailed rules and supported values.

3.  **Scale Resources**
    *   **Upgrade Instance Type (Vertical Scaling)**:
        *   If the instance is consistently under-provisioned despite optimizations, consider upgrading to a larger EC2 instance type with more vCPUs and/or memory.
        *   **Procedure**:
            *   Create a snapshot of your current EC2 instance's root volume (and any data volumes).
            *   Stop the EC2 instance.
            *   Modify the instance type to a larger one (e.g., from `m5.large` to `m5.xlarge`).
            *   Start the instance.
            *   Verify application functionality and CPU utilization.
            *   Alternatively, you can launch a new instance from the snapshot with the larger instance type and then update DNS records or load balancer configurations to direct traffic to the new instance.
    *   **Implement Amazon EC2 Auto Scaling (Horizontal Scaling)**:
        *   For dynamic workloads, Amazon EC2 Auto Scaling is the recommended solution to automatically adjust capacity to maintain performance and optimize costs.
        *   **Create an Auto Scaling Group (ASG)**:
            *   Define a launch template or launch configuration for your instances, specifying the desired instance type, AMI, security groups, and user data.
            *   Create an ASG, specifying the desired capacity (min, max, and desired instances) and the VPC/subnets.
        *   **Configure Target Tracking Scaling Policy**:
            *   Create a target tracking scaling policy for your ASG.
            *   **Metric**: Choose `ASGAverageCPUUtilization`.
            *   **Target Value**: Set a target CPU utilization percentage (e.g., 50-70%) that allows for traffic spikes without over-provisioning.
            *   Amazon EC2 Auto Scaling will automatically add instances when the average CPU utilization exceeds the target and remove instances when it falls below the target.
            *   Refer to [Target tracking scaling policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html) for detailed instructions.

### Verification

After implementing the remediation steps, verify that the CPU utilization has returned to acceptable levels.

1.  **Monitor CloudWatch Metrics**:
    *   Return to the CloudWatch console and view the `CPUUtilization` metric for the remediated EC2 instance(s) or the `ASGAverageCPUUtilization` for your Auto Scaling Group.
    *   Confirm that the CPU utilization is consistently below the threshold (e.g., 90%).
    *   Observe the trend over a period (e.g., several hours or a day) to ensure stability.
2.  **Application Performance**:
    *   Verify that your application is performing as expected, with no signs of slowdowns or errors related to resource constraints.
    *   Check application-specific metrics or logs for any performance improvements.

### Next Steps

To prevent future occurrences of high CPU utilization and maintain a healthy EC2 environment:

1.  **Refine Auto Scaling Policies**: Continuously monitor your application's performance and adjust Auto Scaling policies (target values, instance warmup times, scaling cooldowns) as needed to optimize for cost and performance.
2.  **Implement Comprehensive Monitoring and Alerting**:
    *   Set up CloudWatch alarms for `CPUUtilization` (or `ASGAverageCPUUtilization`) at lower thresholds than the detection script's failing threshold (e.g., 70-80%) to receive early warnings.
    *   Create CloudWatch dashboards to visualize key EC2 metrics, including CPU, memory, disk I/O, and network performance. The [CloudWatch solution: Amazon EC2 health](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Solution-EC2-Health.html) provides a pre-configured dashboard.
3.  **Regular Performance Reviews**: Periodically review your EC2 instance types and application performance to ensure they are appropriately sized for your current workloads. Consider using AWS Compute Optimizer for recommendations.
