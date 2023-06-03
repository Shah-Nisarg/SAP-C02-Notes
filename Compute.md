EC2
=

Types:
- R - RAM
- C - Compute
- M - medium/ general purpose 
- I - I/O storage
- G - GPU
- T - burst

Placement groups
- Spread - High availability
  - max 7 instances per AZ per group
- Cluster - High performance 🚀
  - Same rack
  - 10 Gbps
- Partition - spread with partitions
  - Max 100s of instances
  - Hadoop, cassandra, kafka

How to change placement group?
- Stop
- cli modify-instance-placement
- Start

Launch types
- On demand
- Stop
- Reserved
- Dedicated instances
- Dedicated hosts

Graviton
- Best price performance 🚀💲
- Not available for windows ⛔

Metrics:
- CPU
- Network in/out
- Status checks 
  - Instance - check the VM
  - System - underlying host
  - ⚠️ When EC2 is recovered using instance recovery, same private, public, elastic IP, metadata, placement group is maintained.
- Disk
- ⛔ NOT RAM

Spot fleets
=

- On-demand + Spot
- Set a max price
- Mix of instance types (M, G, R, C, T)
- Up to 100,000 instances
- Strategies:
  - LowestPrice
  - Diversified (across different pools, OS, AZ, Instance types)
  - capacityOptimized (max capacity)

HPC 🚀🚀🚀🚀🚀
-

- Data transfer
  - DX
  - Snowball, Snowmobile
  - Datasync
    - Move large amount of data between on-premises and S3, FSx, EFS
- Compute
  - EC2
  - Cluster placement groups
  - ✅ Enhanced networking (SR-IOV)
    - Higher bandwidth, packets per second, low latency
    - 1. ENA (Elastic Network Adapter) - 100 Gbps
    - 2. Intel 82599 VF - 10 Gbps
  - ✅✅ Elastic Fabric Adapter
    - Only works for linux ⛔
    - Great for inter-node communication in tightly coupled workloads
    - Leverages message passing interface, bypasses linux
- Storage
  - EBS - up to 256k IOPS with io2 block express
  - Instance store - millions of IOPS
  - S3 - blobs
  - EFS - scale IOPS based on size or provisioned IOPS
  - FSx for lustre - millions of IOPS
- Automation and orchestration
  - AWS Batch
    - Easily schedule jobs and launch EC2 instances 
  - AWS Parallel cluster
    - Configure clusters with text files
    - Automatic creation of VPC, subnet, cluster tyupe and instance types

Auto scaling
=

- Target tracking scaling
  - Keep average CPU at 40%
- Simple/Step scaling
  - When CPU > 70%, add 2 units
- Scheduled actions
  - Add 1 units at 12 PM, remove 1 at 8 PM
- Predictive scaling

Useful metrics
-

- CPU
- RequestCountPerTarget ✅
- Average network IO
- Custom cloudwatch metrics

Notes:
-

- Spot fleet is supported (mix of on-demand and spot instances)
- Lifecycle hooks (cleanup, log extraction before termination) ✅
- Upgrade an AMI - modify launch configuration or template
  - Terminate instances manually
  - Or, EC2 instance refresh
    - Recreate all auto-scaling instances using the launch template
    - Specify minimum healthy to avoid disruption
    - warm-up time to allow instances to be ready
- Processes (that can be suspended):
  - Launch
  - Terminate
  - HealthCheck - ex. to stop healthchecks on instance while performing maintenance
  - ReplaceUnhealthy
  - AZRebalance
  - AlarmNotification - ex. accept a notification from cloudwatch
  - ScheduledActions
  - AddToLoadBalancer - ex. to perform actions before instance is added to ALB target group
  - InstanceRefresh
- New instance is launched after terminating unhealthy instances
- Health checks
  - EC2 status checks
  - ELB health checks

Containers
=

- ECS
  - Types
    - EC2
    - Fargate (Serverless) 🖥️
  - Cluster of instances
  - Service = a group of tasks to run
  - Task definition - JSON about how to run a container, i.e. image, CPU, RAM
  - Task - a running docker container
  - IAM role
    - EC2 instance profile
      - Required to register instances with ECS and send logs
    - Task IAM role
      - Required for tasks such as accessing S3
  - ✅ Dynamic port mapping with ALB
  - Networking
    - none - no network, no ports
    - bridge - docker's container based network
    - host - bypass docker, use host network interface
    - ✅ awsvpc - ENI and private IP in VPC
      - with security groups, VPC flow logs
      - default for fargate
  - Auto scaling
    - CPU and RAM are tracked
    - Scaling policies: Step, target tracking, scheduled
    - EC2 instances must be auto scaled separately ⚠️
  - Spot - minimum can be launched using on-demand and rest can be launched using FARGATE_SPOT for cost savings
- ECR 
  - Store images privately or publicly
  - Fully integrated with ECS
  - IAM access control, policy (e.g EC2 instance profile to pull images) ✅
  - ⚠️ Image vulnerability scanning, versioning
    - Results are sent into event bridge
    - **Amazon Inspector** can perform enhanced scanning
  - Cross region and cross account replication
- EKS
  - Managed kubernetes
  - Nodes: AWS Managed EC2, Self-Managed EC2, Fargate
    - EC2 nodes can be on-demand or spot
  - Volumes
    - EBS
    - EFS
    - FSx for NetApp ONTAP
    - FSx Lustre