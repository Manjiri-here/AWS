AWS VPC

1) What is VPC and why it is used?
-> stands for virtual private cloud it allows you to launch aws resources in logically isolated section of AWS cloud. It provides greater control over networking env influding VPC, subnets, routinh tales making it ideal for creating private and secure networks in cloud.

2) What is significance of CIDS notation in VPC?
-> It is classless Interdomain routing. It is range of IP address in VPC. It allows us to specify a range of IP addresses using a combination of an IP addess and a subnet mask. For eg. 10.0.0.0/16 represents a VPC with range of IP address from 10.0.0.0 to 10.0.255.255

3) How are subnets used in VPC?
-> It is cmponent in VPC, it i sused when you ant to devide your IP address range into smaller segments. Subnets are AZ specific. Each subnet must be associated with a specific AZ and resource launched in a asubnet are bund to that AZ. Subnets enable the isolation of resources and help in designing highly available and fault tolerant and secure architecture.

4) What is purpose of VPC's main route table?
-> The main route table in a VPC is adefault route teable associated with all sunets unless explicitly specified otherwise. It controls the traffic leaving the subnets and is a convenient way to manage the default routing behavior for resources in the VPC. WIthout RT VPC wll not eb able to process the tarffic , it  acepts the tarffic and send it to respective subnet,

<img width="719" height="440" alt="Screenshot 2025-11-30 at 12 20 25" src="https://github.com/user-attachments/assets/a252be77-3455-4b0a-b942-b91f6bd164a3" />

5) How does Network Address Translation work in a VPC
-> There are two types of GWs - 1) Internet GW 2) NAT GW
NAT in a VPC allows instances in private subnets to initiate  outbound traffic to the internet while preventing inbound traffic from reaching those instances. 
Used when you want to give internet access to reources in private subnet, it gices only outbound access, inbound traffic is not allowed.

6) Explian difference between VPC peering and a VPN connection.
-> VPC peering allows the connection of two VPCs enabling them to communicate as if they are a part of the same network.

<img width="399" height="206" alt="image" src="https://github.com/user-attachments/assets/cb1e54c2-fbd5-4e71-a49c-9bafa9192cd4" />





   
