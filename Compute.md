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
