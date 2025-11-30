AWS VPC

1) What is VPC and why it is used?
-> stands for virtual private cloud it allows you to launch aws resources in logically isolated section of AWS cloud. It provides greater control over networking env influding VPC, subnets, routinh tales making it ideal for creating private and secure networks in cloud.

2) What is significance of CIDS notation in VPC?
-> It is classless Interdomain routing. It is range of IP address in VPC. It allows us to specify a range of IP addresses using a combination of an IP addess and a subnet mask. For eg. 10.0.0.0/16 represents a VPC with range of IP address from 10.0.0.0 to 10.0.255.255

3) How are subnets/subnetwork used in VPC?
-> It is cmponent in VPC, it i sused when you ant to devide your IP address range into smaller segments. Subnets are AZ specific. Each subnet must be associated with a specific AZ and resource launched in a asubnet are bund to that AZ. Subnets enable the isolation of resources and help in designing highly available and fault tolerant and secure architecture.

4) What is purpose of VPC's main route table?
-> The main route table in a VPC is adefault route teable associated with all sunets unless explicitly specified otherwise. It controls the traffic leaving the subnets and is a convenient way to manage the default routing behavior for resources in the VPC. WIthout RT VPC wll not eb able to process the tarffic , it  acepts the tarffic and send it to respective subnet,

<img width="719" height="440" alt="Screenshot 2025-11-30 at 12 20 25" src="https://github.com/user-attachments/assets/a252be77-3455-4b0a-b942-b91f6bd164a3" />

5) How does Network Address Translation work in a VPC?
-> There are two types of GWs - 1) Internet GW 2) NAT GW
NAT in a VPC allows instances in private subnets to initiate  outbound traffic to the internet while preventing inbound traffic from reaching those instances. 
Used when you want to give internet access to reources in private subnet, it gices only outbound access, inbound traffic is not allowed.

6) Explian difference between VPC peering and a VPN connection.
-> VPC peering allows the connection of two VPCs enabling them to communicate as if they are a part of the same network.

<img width="399" height="206" alt="image" src="https://github.com/user-attachments/assets/cb1e54c2-fbd5-4e71-a49c-9bafa9192cd4" />

VPN connection used when you want to establish secure connection betwen on premises data center and an AWS VPC over the internet.

7) What is an elastic IP(EIP) address and when would you use it in VPC?
-> An elastic IP is a static IPv4 address designed for dynamic cloud computing. In a aVPC, an EIP can be associated with an EC2 instance providing a persistent public IP address that remains the same even if the instance is stopped and started.
EIP are often used for scenarios requiring a fixed public IP, such as hosting a website or application.

8) How can you secure communication between instances in a VPC?
-> Communication between instances in a VPC can be secured using security groups and NACLs. 

Security groups act as stateful firewalls at the instane level while NACLs operate at the subnet level and control traffic at the subnet boundry.

<img width="399" height="345" alt="image" src="https://github.com/user-attachments/assets/70fe8f8a-20ec-4a7b-9c25-b88d9fc67abb" />

9) What is a VPC endpoint and why would you use it?
-> A VPC wndpoint allows instances in your VPC to commmunicate directly with AWS services without traversing the internet. It provides a secure and private connection to services like s3 or DynamoDB improving performance and reducing exposure to potential security risks.
NAT GW does provides us internet connection but it also give access outside AWS, but if you dot want intenet outside AWS then VPC endpoint gives that. It gives you secure and private internet access inside AWS.

<img width="465" height="352" alt="image" src="https://github.com/user-attachments/assets/6b62f826-ce40-4ae4-b211-69c611b0551b" />


10) How do you troubleshoot connectivity issue in a VPC?
-> Troubleshooting connectivity issues in a VPC involves checking route tables , security groups, NACLs and subnet configurations.
If subnet is configuration is proper, if route table has been defined properly.
Tools like VPC flow logs can be enabled to capture and analyse network traffic, helping identify issues related to communication between instances within the VPC or with external resources.

11) What is a default VPC?
12) What are components of AWS VPC? -> VPC, subnets, IGW, NAT GW, route tables, security groups, VPC peering, VPC connections
13) Explian use of route tables/
14) What is Amazon VPC router do?
15) What are IGW in VPC?
16) How do you determine which AZ my subnets are in? -> AWS CLI or AWS management console
17) How many maximum numbe of EC2 instances can you use within a VPC?
-> A max number of AWS instances you can use within a VPC depends on various factors including the type of instance, instance limits, available resources and regional limit set by AWS. These limits can be increased by requesting a limit increase from AWS support.
On demand instances: By default limit of 20 instances per region. SOme specific instance families such as t2.micro and t3.micro may have a higher default limit of 100 instances per region for new accounts.
Additionally a VPC itself has a maximum size of /16 which coresponds to 65536 1p address. This mean that your VPC can accomodate up to 65536-5= 65529, assuming each instance requires only 1 IP address. The  first 4 IP addresses and the last IP address in each subnet CIDR block are not available for you to use and cannot be assigned to an instance. For ex: in a subnet with CIDR block: 10.0.0.0/24 the following 5 IP address are reserved: 
10.0.0.0- Nwtwork address
10.0.0.1: Reserved by AWS for the VPC router
10.0.0.2- Reserved by AWS, teh IP address of the DNS server is the base of the VPC network range plus two.
10.0.0.3- Reserved by AWS for future use
10.0.0.255- Network broadcast address

17) What is classic link?
-> Classic link is specifically designed to connect EC2- classic instances to a VPC. For newer EC2 instances running within a VPC you do not need to use classic link as they are already integrated into the VPC networking env. It is useful featute when you have existing EC2 instances that need to communicate with resources in a VPC, enabling you to levarage the benefits of a VPC while preserving connectivity for older classic instances.
19) How do you create VPC in AWS.
20) Can a VPC CIDR block go beyond /16? Why or why not?
21) How does AWS VPC networking work?
22) What are VPC Peering and Transit Gateway? When would you use them?
23) I’ve noticed even experienced engineers freeze when asked something as basic as: 
- What’s the difference between a public and private subnet? 
- How would you troubleshoot connectivity between subnets? 
- Why can’t your EC2 instance reach the internet? 

The reason: VPC looks “easy” on paper but it’s a foundation that requires clear mental models. Without that, the details blur. 

Let’s break it down. 

Public Subnet: 
A subnet with a route to an Internet Gateway (IGW). EC2 instances here can talk to the internet (if they have public IPs). 

Private Subnet: 
No direct IGW route. To reach the internet, traffic must pass through a NAT Gateway/Instance sitting in a public subnet. 

Troubleshooting Mindset: 
- Start with the route table. Does the subnet point to IGW, NAT, or only VPC CIDR? 
- Check security groups and NACLs. Are they blocking inbound/outbound? 
- Confirm the instance has the right IPs assigned (private vs. public). 
- Trace step by step: Instance → Subnet → Route Table → Gateway. 

The trap in interviews is that people jump to “tool answers” (ping, telnet, traceroute) without explaining the flow of traffic. What interviewers really test is your ability to think in layers: networking, VPC components, and AWS design. 

If you can visualize packets moving, you’ll never forget. 

The lesson: Before diving into advanced AWS services, get your VPC basics solid. Because if you can’t explain public vs. private subnets confidently, how will you design secure multi-region architectures?

24) Steps to setup VPC with subnets and everything
25) 
1. You have a private subnet with no internet access, but your instances need to download updates. How do you enable this securely? -> We use NAT gateway, which helps only outbound traffic and no inbound traffic. So instance or private subnet itself is connected with NAT g/w which is present on public subnet. So instance does not directly communicates with internet, the request goes to NAT g/w and it further communicates with the internet so that IP address of our instance is not exposed. Terraform implementation:
resource "aws_route" "private_internet_access" {
  route_table_id         = aws_route_table.private_rt.id
  destination_cidr_block = "0.0.0.0/0"
  nat_gateway_id         = aws_nat_gateway.nat.id
} 
Q2. Your application in one VPC needs to communicate with a database in another VPC (same region). How would you set this up?
✅ Your answer: By using VPC Peering ✔️ Correct — VPC Peering enables private communication between two VPCs in the same or different AWS accounts, within same or different regions (though cross-region incurs cost).
🧠 Key points:
* Create VPC Peering connection
* Update route tables in both VPCs
* Ensure security groups allow traffic from peered CIDR ranges
📌 Terraform Example:

resource "aws_vpc_peering_connection" "peer" {
  vpc_id        = aws_vpc.app_vpc.id
  peer_vpc_id   = aws_vpc.db_vpc.id
  auto_accept   = true
}

resource "aws_route" "app_to_db" {
  route_table_id         = aws_route_table.app_rt.id
  destination_cidr_block = aws_vpc.db_vpc.cidr_block
  vpc_peering_connection_id = aws_vpc_peering_connection.peer.id
}
🧠 Alternative for many VPCs / better management = Transit Gateway

3️⃣ Q3. You’ve created a new EC2 instance in a public subnet but it cannot access the internet. What might be misconfigured?
⚠️ Your answer: “Outbound rules need to be checked” — partially correct, but incomplete.
✅ Full possible issues:
1. No route to Internet Gateway in subnet’s route table → Must have 0.0.0.0/0 → igw-xxxx
2. No Internet Gateway attached to the VPC
3. EC2 instance lacks public IP
4. Security group outbound rules block traffic (rare, default allows all)
5. Network ACL blocking outbound traffic
🧠 So, check IGW + route table + public IP first.

4️⃣ Q4. You need to connect your on-premises data center to AWS securely. Which VPC feature would you use?
✅ Answer: Use AWS Site-to-Site VPN
* Creates an encrypted IPsec tunnel from your data center to AWS VPC
* Connects via Virtual Private Gateway (VGW)
🧠 For high performance / hybrid workloads:
* Combine with Direct Connect + VPN for redundancy
📌 Terraform resource:

resource "aws_vpn_connection" "onprem_vpn" {
  customer_gateway_id = aws_customer_gateway.onprem.id
  vpn_gateway_id      = aws_vpn_gateway.vpg.id
  type                = "ipsec.1"
}

5️⃣ Q5. How do you restrict access to an EC2 instance so that only a specific IP range can connect?
✅ Your answer: “Create inbound rules mentioning the IPs” ✔️ Correct
📌 Example (Terraform):

ingress {
  from_port   = 22
  to_port     = 22
  protocol    = "tcp"
  cidr_blocks = ["203.0.113.0/24"] # only this range
}
🧠 Always use least privilege (specific IPs, not 0.0.0.0/0).

6️⃣ Q6. Your team wants to host multiple environments (dev, test, prod) in a single AWS account. How would you design the VPCs?
✅ Answer: Use separate VPCs per environment, or one VPC with separate subnets (if isolation isn’t strict).
Best practice:
* Prod → separate VPC for isolation
* Dev/Test → can share a VPC but separate subnets, route tables, security groups
🧠 In large orgs: they often use separate accounts per environment (via AWS Organizations), but one account can work for smaller setups.
📌 Terraform: Create multiple VPCs or parameterize with var.env.

7️⃣ Q7. How would you allow private subnets to access the internet without exposing them directly?
✅ Your answer: By using NAT Gateway ✔️ Perfect
🧠 Ensure:
* NAT Gateway in public subnet
* Private subnet’s route table → 0.0.0.0/0 → nat-xxxx
* IGW attached to VPC

8️⃣ Q8. Your application requires low latency and high throughput between two VPCs across different AWS accounts. What’s the best solution?
⚠️ Your answer: “Create in nearby regions” — ❌ Not correct
✅ Correct answer: Transit Gateway (TGW) or VPC Peering
* Transit Gateway = preferred for multiple VPCs / accounts with high throughput (uses AWS backbone network)
* VPC Peering = okay for one-to-one
🧠 TGW is better for:
* Multi-account setup
* Centralized routing
* Scalable architecture
📌 Terraform:

resource "aws_ec2_transit_gateway" "tgw" {}

9️⃣ Q9. You deployed a NAT Gateway but still can’t reach the internet from private subnets. What could be wrong?
✅ Your answer: “Check route table” ✔️ Correct, but list all common causes:
🧠 Possible misconfigurations:
1. Private route table missing 0.0.0.0/0 → nat-xxxx
2. NAT GW in wrong subnet (must be public)
3. Public subnet missing IGW route
4. Security group / NACL blocking outbound
5. No Elastic IP attached to NAT GW

🔟 Q10. Compliance requires all traffic to pass through a firewall before internet. How would you architect this?
✅ Answer: Use a Firewall Appliance / NAT instance / Network Firewall in a centralized egress VPC.
🧠 Options:
1. AWS Network Firewall (managed)
2. Third-party firewall appliances (e.g. Palo Alto, Checkpoint) in public subnet
3. Use Route Tables to direct all outbound traffic → firewall → IGW
📌 Example route table setup:
* Private subnet → default route → firewall ENI
* Firewall → route to IGW
📌 In Terraform:
* Deploy firewall instance
* Add route:    resource "aws_route" "private_to_firewall" {
*   route_table_id         = aws_route_table.private_rt.id
*   destination_cidr_block = "0.0.0.0/0"
*   network_interface_id   = aws_network_interface.firewall_eni.id
* }
*   
🧠 This is called centralized egress inspection or firewall sandwich architecture.


If you have to design a VPC architecture for a 2 tier application , the application needs to be highly available and scalable. How would you design the VPC architecture?
-> 1) We will create 2 subnets in 2 different regions or in different AZs so if one goes down the other is still up and running.
2. We will use auto scaling groups for servers,

Now the first part of question is how to design 2 tier application- mean application having FE and BE. Its simple. We will create public	and private subnets and will put  load balancer in the public subnet and whole 2tier application in private subnet. Why do we put the application servers in private subnet, the explanation is a below:

The primary reason is enhanced security through isolation:

1. Minimizing the Attack Surface

* No Direct Internet Access: Resources in a private subnet do not have a direct route to the Internet Gateway (IGW). They cannot be directly addressed or accessed from the public internet, even if they had a public IP (which they typically wouldn't be assigned).   
* Protection Against Misconfiguration: If you put your application instances (even just the web layer) in a public subnet, a simple mistake in a Security Group (SG) or a server-level firewall rule could accidentally expose the instance directly to the internet. If the application is in a private subnet, the networking rules (Route Table) act as a fundamental barrier, preventing inbound internet traffic from ever reaching the instance, even with a misconfigured SG.

2. Centralized, Managed Access

* Load Balancer as the Gateway: The Load Balancer (ALB/NLB) acts as the single point of entry for all public traffic. It handles all initial connections, often manages SSL/TLS termination, and performs health checks.
* Security Group Chaining: You configure the Load Balancer's security group to accept traffic from the internet, and then the application instances' security groups are configured to only accept traffic from the Load Balancer's security group. This creates a secure, controlled path for traffic, ensuring only validated requests from the load balancer can reach your servers.  

3. Securing the Backend (Database/Application Logic)

* This practice extends to the entire application. While it is always best practice to put the database in a private subnet, isolating the application/web servers as well ensures that even if an attacker were to compromise a public-facing server (which would be the Load Balancer, a managed AWS service), they still wouldn't have a direct IP to pivot to for the main application instances.


Why the Load Balancer is in the Public Subnet

The Load Balancer (specifically an Internet-facing Load Balancer) must reside in a public subnet because it requires a route to the Internet Gateway (IGW) to accept incoming requests from the internet. The Load Balancer itself is an AWS managed service that handles the public IP address exposure, but it forwards the traffic to the private IP addresses of your application instances in the private subnet.

Q) Your organisation has a VPC with multiple subnets. You want to restrict outbound internet access for resources in one subnet but allow outbound internet access for resources in another subnet. How would you achieve this?
Answer	: to restrict outbound internet access for resources in one subnet we can modify the route table associated with that subnet. In the route table we can remove the default route that points to an internet gateway.
This would prevent resources in that subnet from accessing the internet. For the subnet where outbound internet access is required we can keep the default route pointing to the IGW.

Q) We have VPC with a public subnet and a private subnet. Instances in the private subnet need to access the internet for software updates. How would you allow internet access for instances in the private subnet?
-> By using the NAT GW and routing the outbound traffic of route table for private subnet to the NAT gw in the public subnet. NAT stands for network Address translator, its changes the PI address of EC2 instance in private subnet to its IP before sending the request to internet so the outside sources will not know the IP for private EC2 instance.

Q) you have launched  EC2 instance in your VPC and you want them to communicate with each other using private IP address. What steps would you take to enable this communication?
-> by default instances within the same VPC can communicate with each other using private IP addresses. To ensure this communication we need to make sure that the instances are launched in the same VPC and are placed in the same subnet or subnet that are connected through peering connection or a VPC peering Link. Additionally, we should check that security groups associated with the instances to ensure that the necessary inbound and outbound rules are configured to allow communication between them.(In VPC peering we attach the endpoint of 1st VPC in the route table of 2nd VPC and similarly for the 2nd VPC as well)

Q) You want to implement strict network access control for your VPC resources. How would you achieve this? -> To implement granular network access control for your resources we use network access control list that is NACL. NACLs are stateless and operate at the subnet level. We can define, inbound and outbound rules in the NACLs to allow or deny traffic based on source and destination IP address is ports and protocols. By carefully configuring NACL, else we can enforce fine grained, access control for traffic, entering and leaving the subnets

NACL: 
AN instance happened where there was an attack on EC2 instance where many requests were bumping into it and ultimately the instance was unable to handle it. Upon checking we got to know that these requests were coming from mainly 5 IPs. So to block them we can apply the filter at VPC level itself by applying NACL. NACL help us to configure which IPs can be clocked. (IN case of security we can enable the traffic but cannot specify IPs to block certain traffic.).
Go to EC2-> NACL -> Go inside it and edit it -> We can enter the IPs in inbound rule (Rule number is important here as rule to deny traffic should be of higher priority)

Difference between SG and NACL?
1. NACL is stateless and SG is stateless. Stateful: if you allow one security group
2. SG is on instance level and NACL is on subnet level.
3. We can configure allowed scenarios in SG while in NACL we can configure both allowed and traffic to b denied.  Stateful and stateless :

Feature	Stateful (e.g., Security Groups)	Stateless (e.g., NACLs)
Connection Tracking	Remembers the state of a connection.	Does NOT remember the state of a connection.
Return Traffic	If an inbound request is allowed, the outbound response is automatically allowed, regardless of the outbound rules.	If an inbound request is allowed, the outbound response must be explicitly allowed by a separate outbound rule.
Rule Configuration	Only need to configure a rule in one direction (e.g., inbound).	Must configure rules in both directions (inbound and outbound) to allow two-way communication.
Analogy	Like a doorman who remembers who he let in and lets them leave without re-checking their ID.	Like a customs agent who checks every person's ID coming in and a different agent who checks every person's ID going out, with no shared memory.

Q) Your organisation requires an isolated environment for running sensitive workloads. How would you set up this isolated environment?
-> Subnet does the task of isolation and do not attach any IGW to it. Such type of subnet is called Isolated subnet, as it has no direct internet connectivity. If thissubnet needs outbound access then we can setup a NAT twin different subnet and connected this subnet to that NAT gw.

Q) Your application needs to access AWS services such as S3 securely within your VPC. How would you achieve this.
-> To securely access AWS endpoint within the VPC,  we can use VPC endpoint. It allows instances in VPC to communicate with AWS services privately	without requiring internet or NAT gateways. We can create VPC endpoints for specific AWS services such as S3 or dynamodb and associate them with the VPC. This enables secure and efficient communication.

Q) What is difference between NACL and SG. Explain with a use case.
-> For example I want to design  a security architecture, I would use a combination of NACL and SG. At the subnet Leven I would configure NACL to enforce inbound and outbound traffic restrictions based on source and destination IP address, ports and protocols. NACLs are stateless and can provide an additional layer of defence by filtering traffic at the subnet boundary.
At the instance level, I would leverage security groups to control inbound and outbound traffic. SG are stateful and operate at instance level. By carefully	 defining SG rules, I can allow or deny specific traffic to and from the instance based on the application’s security requirements. 
By combining NACLs and SG, I can achieve granular security controls at both the n/w and instance level, providing defence in depth for the sensitive app.

Q) * How would you use Terraform to provision EKS and VPC together?
Q) How to create VPC peering, how it’s working.
Q) For each AWS account how many VPC’s we can create..?
Q) : Your EKS cluster can’t scale new pods due to insufficient IPs. How do you resolve this?
A: The VPC’s CIDR block might be too small. I’d enable VPC CNI’s custom networking or add a secondary CIDR to increase the IP pool.





   
