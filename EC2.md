EC2 

EC2

You launched an EC2 instance, but it’s stuck in the “pending” state. What’s going on?
Answer: “pending” means AWS accepted your request but the instance is failing to finish provisioning. Something in your config is blocking it.

When you launch an Amazon EC2 instance, AWS does all of this before it becomes running:
Picks physical host
Attaches virtual network interface
Attaches EBS root volume
Assigns private IP
Applies security groups
Boots the OS
If it gets stuck in pending, it’s failing in steps 2–5.

The REAL Reasons It Gets Stuck in Pending
1️⃣ You Picked an Invalid Subnet / Broken VPC
What to check:
Subnet still exists
Subnet has free IPs
Subnet AZ matches instance type
If subnet IPs = 0 → instance will stay pending forever
2️⃣ Capacity Issue in the Availability Zone - No hardware left for your instance type. Try different instance type and AZ
3️⃣ Broken or Unavailable AMI
4️⃣ Security Group / Network Interface Failure
If your network interface fails to attach:
Invalid security group
SG from wrong VPC
Too many ENIs already attached (rare)
5) Account-Level Limits (Silent Killer)
Check:
EC2 → Limits → Running instances
Network Interfaces
EBS volumes

2)  What is User Data in EC2?
Answer: User Data in Amazon EC2 is a startup script (shell script or cloud-init config) that is automatically executed the first time the instance boots. It is mainly used for:
Installing packages
Configuring the OS
Starting services
Bootstrapping applications
User Data in EC2 is a startup script that runs automatically on the first boot of the instance(and not on every restart). It is used to install software, configure the system, and start applications without manual login.



3)  How do you upload/download to S3 bucket privately from EC2?
Answer:

4) Can you write a Terraform script? Please write a Terraform script for EC2.
Answer: Go to VS code and write code. Below is my snippet, for details refer github repo for terraform.

provider “aws” {
  region= “us-east-1”
}

resource “aws_instance” “demo”{
  Ami = “”
  Instance_type = “”
  Subnet =
  Key = 
}


5) Launch an instance and deploy a small app while discovering security groups, SSH keys, and why you need load balancers.


6) What are different EC2 instance types and when to use them?
Answer: ✅ First, the Correct Classification for Amazon EC2
There are TWO independent dimensions:

✅ A) INSTANCE TYPES (Hardware – WHAT kind of machine)
This is about CPU, RAM, Storage, GPU.
1️⃣ General Purpose (T, M)
👉 Balanced CPU + RAM
 ✅ Use for: web apps, APIs, small databases, dev servers
 🧠 Think: “T-M = Typical Machines”

2️⃣ Compute Optimized (C)
👉 High CPU power
 ✅ Use for: video encoding, batch processing, gaming servers
 🧠 C = CPU

3️⃣ Memory Optimized (R, X)
👉 Huge RAM
 ✅ Use for: in-memory DBs, Redis, SAP, real-time analytics
 🧠 R = RAM

4️⃣ Storage Optimized (I, D)
👉 Super-fast disks
 ✅ Use for: Elasticsearch, Kafka, big data
 🧠 D = Disk

5️⃣ Accelerated (P, G, Inf)
👉 GPU / AI hardware
 ✅ Use for: ML, deep learning, graphics rendering
 🧠 G = GPU

✅ B) INSTANCE PRICING TYPES (HOW you pay)
This is what you listed, but you didn’t know it’s a different category.

1️⃣ On-Demand
👉 Pay per second
 ✅ Use: testing, unpredictable traffic
 🧠 On = Open whenever needed

2️⃣ Reserved
👉 1–3 year commitment → cheaper
 ✅ Use: production, steady workloads
 🧠 Reserved = Regular workload

3️⃣ Savings Plan ✅ (You misspelled this as "sayings")
👉 Discount based on usage commitment
 ✅ Use: when flexible between instance families
 🧠 Savings = Smart reservation

4️⃣ Spot Instances
👉 70–90% cheaper but can die anytime
 ✅ Use: batch jobs, CI/CD, data processing
 🧠 Spot = “Spare capacity”

5️⃣ Dedicated Host
👉 Full physical server only for you
 ✅ Use: licensing issues, compliance, banks
 🧠 Dedicated = Don’t want neighbors

✅ THE RIGHT WAY TO ANSWER IN INTERVIEW
EC2 instances are classified in two ways: by hardware type such as General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, and GPU instances. And by pricing model such as On-Demand, Reserved, Savings Plan, Spot, and Dedicated Hosts.

7) Can we attach an S3 bucket to an EC2 instance directly?
Answer: S3 is:
Object storage
API-based
Not a filesystem
No inode, no POSIX locking, no random write
So if you try to treat S3 like a normal disk → your design is already broken.
✅ What You ACTUALLY Do Instead (Correct Way)
You ACCESS S3 from EC2 using:
✅ 1. IAM Role (Identity-based access)
No access keys. No secrets in code.
✅ 2. S3 VPC Endpoint (Private networking)
So traffic does NOT go over the internet.
✅ 3. AWS CLI / SDK
aws s3 cp file.txt s3://my-bucket/
aws s3 sync ./data s3://my-bucket/data
This is secure, private, and production-grade.
No, we cannot attach an S3 bucket to EC2 like a disk because S3 is object storage, not block storage. Instead, we access S3 privately using an IAM role attached to EC2 and an S3 VPC endpoint, and interact with it using the AWS CLI or SDK.
✅ Mental Rule to Remember Forever
EBS = Attach
S3 = Access
EFS = Mount
8) Why can’t your EC2 instance reach the internet? 
Answer: An Amazon EC2 reaches the internet only if ALL of the following are correct. Miss even one → no internet.
✅ 1. Is the instance in a PUBLIC subnet?
If it’s in a private subnet, it cannot talk to the internet directly.
Check:
Subnet route table → Does 0.0.0.0/0 point to an IGW?
✅ 2. Is there an Internet Gateway attached to the VPC?
Your Amazon VPC must have an Internet Gateway attached.
No IGW = zero internet, guaranteed.
✅ 3. Does the route table send traffic to the IGW?
This route must exist:
0.0.0.0/0 → igw-xxxxxxxx.
If it points nowhere → instance is isolated.
✅ 4. Does the EC2 have a PUBLIC IP?
No public IP = no incoming/outgoing internet.
Check:
Auto-assign public IPv4 = ENABLED
Elastic IP attached
✅ 5. Are Security Group OUTBOUND rules open?
Your Security Group must allow egress:
Outbound: All traffic → 0.0.0.0/0
If outbound is blocked → no internet.
✅ 6. Are NACL rules blocking traffic?
Your Network ACL must allow:
Outbound ephemeral ports (1024–65535)


Inbound return traffic
NACL is stateless. One wrong rule → traffic dies.
✅ 7. If it’s in a PRIVATE subnet → Is there a NAT Gateway?
Private subnet needs a NAT Gateway.
Traffic path must be:
EC2 → Route Table → NAT Gateway → Internet Gateway → Internet
No NAT = no internet.
✅ 8. Is DNS resolution working?
If:
ping google.com ❌
ping 8.8.8.8 ✅
Then:
VPC DNS resolution is broken


Route 53 resolver disabled
✅ One-Line Interview Answer
An EC2 can’t reach the internet if it’s missing a public IP, internet gateway, correct route table entry, outbound security group rule, NAT gateway (for private subnets), or if NACL/DNS is misconfigured.
Troubleshooting Mindset: 
- Start with the route table. Does the subnet point to IGW, NAT, or only VPC CIDR? 
- Check security groups and NACLs. Are they blocking inbound/outbound? 
- Confirm the instance has the right IPs assigned (private vs. public). 
- Trace step by step: Instance → Subnet → Route Table → Gateway. 
9) Describe the key difference between EC2 and AWS lambda? When would you choose 1 over another?
Answer: ✅ Core Difference in One Line
EC2 = You manage servers.
 Lambda = AWS manages servers, you only run code.
That’s the entire mental model. Everything else is a consequence of that.
✅ EC2 (Virtual Machine Model)
What it actually is:
A 24/7 running virtual server


You manage:


OS
Patching
Security hardening
Scaling
Crashes
Disk
Memory
When you MUST choose EC2:
✅ Long-running apps (APIs, websites)
✅ Databases
✅ Stateful services
✅ Custom OS/kernel
✅ Background workers that never stop
✅ Legacy apps
What you pay for:
Time (per second/hour)
 Even if the app is idle → you still pay
✅ Lambda (Serverless Execution Model)
What it actually is:
A function that runs only when triggered


You do NOT manage:


Server
OS
Autoscaling
Availability
Patching
When you MUST choose Lambda:
✅ Event-driven tasks
✅ File upload processing
✅ Cron jobs
✅ APIs with unpredictable traffic
✅ Automation
✅ Glue code between services


What you pay for:
Execution time + number of requests
 If it doesn’t run → you pay nothin
✅ Side-by-Side Brutal Comparison
Feature
EC2
Lambda
Server management
❌ You manage
✅ AWS manages
Runs continuously
✅ Yes
❌ No (only on trigger)
Scaling
Manual / ASG
Automatic
Billing
Per hour/second
Per execution
Max runtime
Unlimited
15 minutes
OS access
Full SSH
None
Best for
Apps, DBs, services
Events, automation

✅ When to Choose Which (Decision Rule)
✅ Choose EC2 when:
App must run continuously
Needs local disk
Needs custom networking
Needs GPU / high RAM
Needs full OS control
✅ Choose Lambda when:
Work is event-based
Needs to scale instantly
Must be cheap at low traffic
No need for persistent state
No need for long execution
❌ Common Interview-Killer Mistakes
If you say:
❌ “Lambda is cheaper than EC2” → Not always
❌ “Lambda replaces EC2” → Wrong
❌ “Lambda is for big applications” → Wrong
❌ “You can SSH into Lambda” → Laughable


✅ Perfect 2-Line Interview Answer (Use This)
EC2 is a virtual server where we manage the OS and runtime and use it for long-running applications. Lambda is a serverless compute service where we only run code on events without managing servers, and we use it for short, event-driven tasks.


1. EC2 is running but SSH is not working — What do you check?
First time reference: Amazon EC2.
Step-by-step kill-shot order:
Security Group – Is port 22 open to your IP?
 → If not, SSH is impossible. Period.


Network ACL – Stateless firewall may be blocking inbound/outbound.


Public IP – No public IP = no SSH from internet.


Route Table – Subnet must point 0.0.0.0/0 to an Internet Gateway.


Key Pair – Wrong .pem = dead access.


Instance OS firewall – ufw or iptables might block port 22.


CPU at 100% – Instance may be frozen.


Wrong username – ec2-user, ubuntu, admin mismatch kills access.


Memory rule:
 👉 SSH = Security Group + Public IP + Correct Key + Correct User.

2. You deleted an EC2 by mistake — How do you prevent this again?
Correct answer:
Enable Termination Protection on the instance.


Backup layer:
Use AMI backups


Use EC2 Auto Scaling (so deleted instance auto-recreates)


Hard truth:
 If termination protection was OFF and no AMI exists → instance is gone forever.

3. EC2 must access S3 securely — Best method?
First reference: Amazon S3
 First reference: AWS IAM
Correct architecture:
✅ IAM Role attached to EC2
 ❌ Never store access keys inside the server
Why?
Keys leak


Roles rotate automatically


Zero hardcoding


Interview line:
Attach an IAM Role with S3 permissions. No access keys.

4. App must be highly available across AZs — How?
First reference: Elastic Load Balancing
 First reference: EC2 Auto Scaling
Required design:
Minimum 2 Availability Zones


EC2 inside Auto Scaling Group


Application Load Balancer in front


If one AZ dies → traffic shifts → app stays live.
No ALB + No ASG = Not highly available.

5. Run scripts daily on EC2 — Options?
3 Valid methods:
Linux cron job – Best & simplest


EventBridge + SSM Run Command – For automation without SSH


Systemd timer – Cron alternative


Memory:
 Daily task on EC2 = cron first choice.

6. Auto-scale EC2 based on CPU — How?
First reference: Amazon CloudWatch
Exact flow:
Create Launch Template


Create Auto Scaling Group


Add CloudWatch Alarm


CPU > 70% → scale out


CPU < 30% → scale in


ALARM → ASG → NEW INSTANCES

7. High latency between EC2 and RDS in same region — Why?
First reference: Amazon RDS
Likely causes:
They’re in different AZs


App connects via public IP instead of private IP


Security Group rule delays


DNS resolution issue


Underlying mis-sized instance


Fix: Put EC2 and RDS in same AZ + use private endpoint.

8. Move EC2 to another region — Options?
You cannot move directly.
Only correct method:
Create AMI


Copy AMI to new region


Launch from copied AMI


Alternative:
Snapshot → Copy snapshot → Create volume → Build instance


No export/import shortcut exists.

9. Disk keeps filling — How do you debug and fix?
Investigate:
df -h       # disk usage
du -sh /*   # folder sizes

Common causes:
Logs filling /var/log


Docker images


Old app temp files


Fix:
Rotate logs (logrotate)


Expand EBS volume


Clean unused Docker images


If you ignore disk full → instance WILL crash.

10. Reduce EC2 cost without hurting performance — How?
Correct tools:
Reserved Instances / Savings Plans


Right-size instance


Spot Instances for non-prod


Auto Scaling


Wrong thinking:
“Just shut down servers” → breaks production.



11. App requires very high IOPS — Which storage?
Correct answer:
✅ Provisioned IOPS SSD (io1 / io2)
❌ gp2/gp3 = medium performance
 ❌ HDD = slow
Databases need IOPS. Logs don’t.

12. Security Group updated but traffic still blocked — What else?
Check:
NACL


Route Table


Instance OS firewall


Correct port listening


Target health in ALB


Security Group alone is not the full network.

13. Allow only office IP for SSH — How?
Security Group rule:
Inbound:
Port: 22
Source: <OFFICE_PUBLIC_IP>/32

Never use:
0.0.0.0/0

That is asking for a breach.

14. EC2 should auto-recover if hardware fails — Best solution?
✅ EC2 Auto Recovery (CloudWatch + Recover Action)
 or
 ✅ Auto Scaling Group with min=1
Manual reboot = amateur move.

15. Load balancing across multiple EC2 — How?
Deploy Application Load Balancer


Register EC2 as targets


Health checks enabled


Traffic auto-distributed


Without LB → one instance dies → users die with it.

16. Monitor EC2 & set alerts — How?
Use:
CloudWatch metrics (CPU, Disk, Network)


CloudWatch Alarms


Alerts via SNS Email / SMS


Monitoring without alarms = fake monitoring.

17. Share AMI with another AWS account — How?
Steps:
Open AMI


Modify Image Permissions


Add other AWS Account ID


If encrypted → also share KMS key.

18. Web app must handle traffic spikes — Design?
You need:
ALB


Auto Scaling Group


Stateless EC2


Cache layer (Redis/CloudFront)


If your app stores sessions on local disk → scaling will BREAK it.

19. Spot instance keeps terminating — How to handle?
Solutions:
Mixed ASG: Spot + On-Demand


Use Spot Fleet


Add graceful shutdown scripts


Store everything in S3/EBS, not local disk


Spot = cheap
 Spot = unreliable
 That’s the tradeoff. Accept it or don’t use it.

20. Weekly batch job without running EC2 24/7 — Best method?
Best solutions:
✅ EventBridge → Lambda → Start EC2 → Run Job → Stop EC2
 ✅ Or EventBridge → SSM Run Command (no SSH)
Worst approach:
 ❌ Keep EC2 running all week “just in case”
That is pure cost leakage.

FINAL INTERVIEW MEMORY SHEET
Problem
One-Line Smart Answer
SSH fails
SG + Public IP + Key
Accidental delete
Termination Protection
S3 access
IAM Role
High availability
ALB + ASG + Multi-AZ
Daily task
Cron
Auto scaling
CloudWatch Alarm
RDS latency
Same AZ + Private IP
Region move
AMI copy
Disk full
df + logs + resize
Cost cut
Savings + Spot + Right size
High IOPS
io1 / io2
Traffic blocked
NACL + Firewall
Office SSH
/32 IP rule
Auto recovery
ASG / Recovery Alarm
Load balance
ALB
Monitoring
CloudWatch + Alarms
AMI share
Image permissions
Traffic spikes
ALB + ASG
Spot termination
Mixed instances
Weekly jobs
EventBridge + SSM

Q) How do you allow EC2 to securely access S3 without using access keys?
Attach an IAM Role to the Amazon EC2 with permissions to access Amazon S3. Do NOT use access keys. Ever.

The Only Correct Way (Step-by-step, no nonsense)
Create an IAM Role


Service: EC2


This role will be assumed automatically by the instance.


Attach an S3 Policy to the Role


Example:


AmazonS3ReadOnlyAccess (read only)


Or a custom least-privilege policy for one bucket


Attach the Role to the EC2 Instance


At launch, or modify → Attach IAM Role


Use S3 Directly — No Keys Needed


AWS SDK & CLI auto-fetch temporary credentials from AWS IAM


No aws configure


No access key


No secret key



Why Access Keys Are a Bad Idea (Brutal Truth)
If you store access keys on EC2:
✅ They will leak


✅ They won’t rotate


✅ You will fail security audits


✅ You deserve the breach when it happens


IAM Roles:
✅ Auto-rotating credentials


✅ Short-lived tokens


✅ Zero secrets on disk


✅ Fully auditable



Interview One-Liner (Memorize This)
“Attach an IAM Role with S3 permissions to the EC2 instance. AWS automatically provides temporary credentials—no access keys are required.”

Q) How do you ensure that developers can only launch specific EC2 instance types?
Answers:
The Only Correct Enforcement Method
You do NOT trust guidelines.
 You do NOT rely on tags or naming.
 You HARD-BLOCK with IAM.
You add a condition on:
ec2:InstanceType


Example: Allow Only t3.micro and t3.small
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": [
            "t3.micro",
            "t3.small"
          ]
        }
      }
    }
  ]
}

What this does:
✅ Developers can launch only these two types


❌ Any m5, c7g, r6i → HARD DENIED


❌ No excuses, no overrides, no cost explosions



If You Don’t Do This, What Will Happen?
Be honest with yourself:
Someone will launch a giant instance


Your AWS bill will spike


Management will blame DevOps


And you’ll have zero defense



Enterprise-Grade Control (Stricter)
On top of IAM:
✅ Use Service Control Policies (SCPs) at Org level


✅ Lock instance types for entire accounts


That prevents even admins from bypassing it.

Interview Kill Line (Memorize This)
“We restrict allowed EC2 instance types using an IAM policy condition on ec2:InstanceType, optionally enforced at the Organization level with SCPs to prevent cost escalation.”
Q3. You’ve created a new EC2 instance in a public subnet but it cannot access the internet. What might be misconfigured?
⚠️ Your answer: “Outbound rules need to be checked” — partially correct, but incomplete.
✅ Full possible issues:
No route to Internet Gateway in subnet’s route table
→ Must have 0.0.0.0/0 → igw-xxxx
No Internet Gateway attached to the VPC
EC2 instance lacks public IP
Security group outbound rules block traffic (rare, default allows all)
Network ACL blocking outbound traffic
🧠 So, check IGW + route table + public IP first.

 Q5. How do you restrict access to an EC2 instance so that only a specific IP range can connect?
✅ Your answer: “Create inbound rules mentioning the IPs”
✔️ Correct
📌 Example (Terraform):
ingress {
  from_port   = 22
  to_port     = 22
  protocol    = "tcp"
  cidr_blocks = ["203.0.113.0/24"] # only this range
}
🧠 Always use least privilege (specific IPs, not 0.0.0.0/0).

Q: Can you explain the difference between Elastic Beanstalk and EC2?
A: Elastic Beanstalk automates infrastructure deployment for web apps while EC2 provides raw virtual servers to manage manually.
Q). How to connect EC2 instance my personal laptop without allowing any public permissions.
Answers: First reference: AWS Systems Manager
What this gives you:
✅ No public IP


✅ No SSH ports


✅ No VPN


✅ No bastion


✅ Fully audited


✅ Works from your laptop via AWS CLI


How it works (exact flow):
EC2 has no public IP


EC2 gets an IAM Role with:


AmazonSSMManagedInstanceCore


SSM Agent runs on the instance


You connect like this:

 aws ssm start-session --target i-xxxxxxxx


Why this is the smartest answer in interviews:
Zero exposed ports


Zero inbound internet traffic


Zero key management


Zero attack surface


If security is your goal, this is the top-tier solution.

✅ OPTION 2: VPN → Private IP → SSH
Used when you actually need network-level access.
Flow:
Create Client VPN or Site-to-Site VPN


Your laptop connects to the VPN


You SSH using private IP:

 ssh ec2-user@10.0.2.15


Requirements:
EC2 in private subnet


No public IP


Security Group allows SSH only from VPN CIDR


This is real enterprise networking, not hobby setup.

❌ WHAT YOU MUST NOT DO
Bad Practice
Why You Fail
Public IP + 0.0.0.0/0
Open to global hackers
Storing PEM in Git
Security breach
Bastion with open SSH
Still public exposure
Port forwarding
Sloppy & risky

If you do any of this in production → you're not doing security, you're gambling.

✅ INTERVIEW PERFECT ANSWER (MEMORIZE)
“To connect securely without public access, I either use AWS SSM Session Manager with an IAM role and zero open ports, or I connect through a VPN and access the EC2 using its private IP.”

Decision Table (So You Don’t Think Emotionally)
Situation
Correct Choice
Max security
✅ SSM Session Manager
Full network access needed
✅ VPN + Private IP
Quick & dirty lab
❌ Public IP (only for learning)



