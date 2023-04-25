Public vs Private Services
=

Public Internet - e.g. Gmail
Private Internet - AWS
  - Public Services - e.g. S3
  - Private Services - e.g. EC2 (inside VPC)

DHCP
-
Dynamic Host Configuration Protocol - it is how EC2 instances get IP address and DNS names.

Properties 
 - Immutable
 - Creates new versions if it is modified
 - Max 1 per VPC
 - It gets linked to a VPC immediately, but changes take effect when it renews

DNS Names:
 - AWS Provides the default DNS names if _EnableDNSHostNames_ is true for VPC
 - If the EC2 has public IP, it gets a public DNS name
 - If it has private IP, it gets a private DNS name
 - Custom DNS name can be assigned using custom Route 53 domain

➡️Subnet IP + 1 = VPC Router
➡️VPC + 2 = Route 53 resolver

VPC Router
-

- Present in each subnet at Subnet IP + 1 (192.168.0.1)
- Routes all traffic that is leaving the subnet, e.g. to other subnets, in other AZ, to internet, to on-prem networks
- Uses 1 route table to decide routing

Route table:
- The most specific route wins
- If a custom route table is disassociated from a subnet, default route table is associated with it‼️

Stateful vs Stateless Firewalls
=
Stateless:
- e.g. NACL
- Operates at layer 3, doesn't understand TCP
- Require rules to permit both outbound and inbound traffic for a request
- Outbound: www.google.com:443
- Inbound: 1.2.3.4:12345 (Ephemeral port)⚠️
- High admin overhead due to both sides of rules

Stateful:
- e.g. security groups
- Operates at layer 4-5, understands TCP
- If request is allowed, response is also allowed
- Less admin overhead
- No need to enable ephemeral ports as they are only required for response

**NACL**:
- Controls traffic entering or exiting **subnets**
- Default NACL in a VPC allows all traffic
- Custom NACL blocks all traffic
- Lowest rule (1) has highest weight
- Can deny a specific IP address

**Security Groups**:
- Controls traffic entering or exiting an **ENI**‼️, not EC2
- Cannot deny traffic
- Cannot deny a specific IP address
- Can reference other security groups or logical resources
-   New instances launched in the ASG inherit the security groups, thus they inherit the permission to interact with other SGs that allow traffic from the source SG

AWS Local Zones
=

- A zone near your on-prem data center
- Better performance, less latency 🕐
- Supports Direct Connect
- Supports limited services
- Uses the nearest region for some services such as S3, EBS snapshots

AWS Global Accelerator
=

 - Brings AWS network closer to the customer
 - Creates two anycast IP addresses
 - When customer hits these IP, they enter AWS backbone network via the nearest Global Accelerator location
 - Traffic is routed to the nearest location of AWS infrastructure of the application 
 - Example: User is in london and infrastructure is in Ireland and US East - then user's traffic enters the AWS private netwrok in London and it is routed to Ireland via AWS private network.
 
 Different from Cloudfront ⚠️:
  - No caching
  - Supports TCP, UDP (non-HTTPS traffic)
  

Site 2 Site VPN
=

 - Connects AWS VPC with On-prem network
 - Uses IPSec to encrypt the traffic traversing over public internet
 - Takes 1 hour to setup
 - 1 VGW Virtual Private Gateway + 1 CGW Customer Gateway
 - VGW is highly available by default, it has two interfaces in two separate AZs
 - CGW is a single device on-prem, **NOT** highly available
 - To make CGW HA, another CGW can be created on-prem and linked as another VPN connection on the same VGW
   - 1 VGW (4 interfaces) + 2 CGW = HA VPN
 - CGW requires public IP Address
 - Route table must be modified with CIDR range of the networks to route traffic correctly
 - VGW is in the AWS public zone ✅ (not in VPC)
 - Up to 1.25 Gbps (limit of AWS) ⚠️
 - Travel over public internet is inconsistent and high latency ⚠️⚠️
 - AWS Cost - Per GB, per hour 💲💲
 - Can be laid over direct connect for encryption

Static VPN = networks are statically configured
 - No load balancing
 - No failover

Dynamic VPN = Uses BGP
  - Network info exchanged
  - Multiple VPN provide HA
  - Route propagation allows dynamically updating the route tables
