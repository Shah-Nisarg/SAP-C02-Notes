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

Accelerated S2S VPN
-

**S2S VPN + AWS Global Accelerator**
- Only works with transit gateway attachment
- Does not work with VGW 🚫🚫
- TGW VPN attachment gets 2 anycast IP addresses which route the traffic to global AWS extended network
- The traffic enters the AWS network through the nearest global location
- Reduces latency and jitter, provides high performance and stable connection to TGW
- Cost = Fixed cost for accelerator + Data transfer fee 💲💲
- ✅ Transit gateway is usually preferred over VGW because of the ability to link multiple VPC and integration with AWS Global accelerator

**Client VPN**

- S2S VPN = On-prem to AWS VPC
- Client VPN = Client Desktops to AWS VPC
- Implementation of open VPN
- Creates an interface in the target subnet for VPN connection
  - Use multiple subnets for high availability
- Replaces the entire route table on the client machine 
  - Any connections to internet go through the client VPN through NAT gateway or internet gateway
  - Connections to other VPC can go through VPC Peering connection
  - Clients connect with each other through the VPN connection
- Cost - fixed cost + cost per subnet + cost per hour per connection

**Client VPN - Split tunnel**

- Instead of replacing the client route table, adds routes
  - Internet traffic flows directly out to internet
  - Clients can communicate on local network
- Not enabled by default

🤯 Transit Gateway
=

Without transit gateway
 - VPC must be connected in mesh topology
 - Each VPC must have VGW to connect using S2S VPN with on-prem
 
 With transit gateway
  - Can intelligently route traffic between multiple VPCs
    - (It is a router)
  - VPC, S2S VPN and Direct connect can connect with 1 Transit gateway
  - Single region device, but it can **peer** with other accounts in same or other regions
    - 1 TGW can peer with up to 50 TGW
    - Each of the peered TGW can peer with up to 50 other TGW 🤯
    - BGP and route propagation **doesn't** work with peering
  - Can be shared using AWS RAM (Resource Access Manager)
  - Route tables need to configured
    - Can be populated using BGP (Border Gateway Protocol) or static
  - Reduces network complexity
  - Highly available and scalable ✅
  - ⭐ It can also have VGW to terminate VPN traffic
    - So the VPCs do not have to have separate VGW

Note ✅:
  - Public DNS name to private IP address is not supported by transit gateway
    - because the traffic may be cross region
    - But VPC does support it
  - Cross region data is encrypted

Routing ⭐:
  - Association
    - A route table associated with a TGW **attachment** (i.e. a link with either a VPC, VPN VGW or Direct Connect VIF Gateway)
    - Route table is used to determine where the traffic exiting from an **attachment** should go
      - that is reviewed from the route table associated with the **attachment**
  - Propagation
    - Through BGP route table is updated if an attachment is permitted to propagate its route to that route table
    - So if a DX's or VPN's IP changes, the route table is automatically updated
  - Allows creating isolated networks, e.g. 2 VPC with 1 VPN and 1 DX
    - VPCs cannot interact with each other, but they can interact with the DX and VPN
    - DX and VPN can interact with either of the two VPC


Route table
=

- Up to 50 static routes, 100 dynamic routes
- 1 subnet, max 1 RT

Rules Priority Order
-
1. Most specific aka. Longest prefix (/32, /24, /16, /0)
2. Static routes
3. Dynamic routes (i.e. routes learned by VGW)
4. Dynamic routes from DX
5. Dynamic routes from VGW - static configuration
6. Dynamic routes from VGW - BGP
7. Dynamic routes learned with AS_PATH - shortest hop length


Gateway Route table
- 
Allows routing traffic to a security appliance as it enters the VPC.

Links to gateways such as,
1. VGW
2. IGW Internet Gateway


Direct Connect
=

- Physical connection from on-prem to AWS
- 1, 10, 100 Gbps
- Allocation may take up to a month
- Unencrypted connection ☠️
- May connect to VPC or AWS public zone using private or public VIF
- Only Fiber, no copper
- Connects to physical port
- Auto negotiation is disabled, Port speed and full duplex needs to be manually configured
- Uses BGP and BGP MD5
- AWS doesn't own the DX location where AWS port is connected to customer's port
- DX is a layer 2 connection, multiple layer 3 connections can happen on DX using VIF, VLAN and BGP


VIF
-

- 50 public + private VIF on dedicated DX
- 1 VIF on hosted DX
- BGP happens between customer DX router and AWS DX router

Private VIF
-

- 1 VIF = 1 VGW = 1 VPC in the _**same region**_
- No encryption
- Can use 1500 bytes or 9000 bytes frame (Jumbo frames)
- VIF can terminate either on VGW or DGW Direct connect gateway
- AWS advertizes VPC CIDRs and BGP peer IPs
- You can advertize corp specific prefixes up to 100
  - If more than 100 prefixes are advertized, it breaks 😱😱😱
- Only allows connecting to private IPs of your EC2

Public VIF
-

- For connecting to AWS public zone services
- AWS advertizes AWS Public IP ranges over BGP
  - Allows connecting to elastic IP of your EC2
  - Also, allows connecting to elastic IP of other customers ⚠️⚠️
- AWS allows connection to AWS public zones in _**other regions**_ ✅


Encryption options
-

**IP Sec VPN**
- Provides end to end encryption
- Reduces performance due to encryption overhead
- Works over DX connection using a **public VIF** to connect with TGW or VGW in AWS Public zone ‼️
  - Very quick to provision due to wider support
  - VIF + VPN provides low and consistent latency
  - Also allows DX to connect to public zone of other regions 🌍

**MacSec**
- Provides encryption for each hop at layer 2
- Physical connection is requried between the two devices
- High performance
- Difficult to find network devices supporting MacSec
