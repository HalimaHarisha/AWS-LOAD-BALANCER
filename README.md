# AWS-LOAD-BALANCER


<h1>AIM</h1>
To use Elastic Load Balancing (ELB) and Auto Scaling services to load balance and automatically scale an AWS infrastructure.

<h1>PROBLEM STATEMENT</h1>
As web applications receive varying levels of traffic, a single EC2 instance cannot reliably handle sudden spikes in demand, nor can it provide fault tolerance if that instance fails. Elastic Load Balancing (ELB) solves this by automatically distributing incoming application traffic across multiple EC2 instances and Availability Zones, ensuring high availability and fault tolerance.

However, load balancing alone does not solve the problem of capacity — if traffic increases beyond what the current instances can handle, more instances are needed; when traffic decreases, running excess instances wastes cost. AWS Auto Scaling addresses this by automatically launching or terminating EC2 instances based on defined conditions (such as CPU utilization), maintaining application performance while minimizing cost.

This experiment demonstrates how to create a reusable Amazon Machine Image (AMI) from a running server, configure an Application Load Balancer to distribute traffic, set up a Launch Template and Auto Scaling group to automatically manage instance capacity, and use CloudWatch alarms to trigger and verify scaling behavior under load.

<h1>ALGORITHM</h1>
Step 1: Create an AMI for Auto Scaling
Open the EC2 console, confirm that Web Server 1 is running (2/2 status checks passed), select the instance, and choose Actions → Image and templates → Create image. Name it "WebServerAMI" and create it. This AMI will be used to launch identical instances later.

Step 2: Create a Target Group and Load Balancer
Create a Target Group named "LabGroup" (type: Instances, VPC: Lab VPC) without registering targets yet. Then create an Application Load Balancer named "LabELB" under Lab VPC, mapped to Public Subnet 1 and Public Subnet 2, using the Web Security Group, with the HTTP:80 listener forwarding to LabGroup.

Step 3: Create a Launch Template and Auto Scaling Group
Create a Launch Template named "LabConfig" using the WebServerAMI, instance type t2.micro, key pair "vockey", the Web Security Group, and Detailed CloudWatch monitoring enabled. Using this template, create an Auto Scaling group named "Lab Auto Scaling Group" attached to Private Subnet 1 and Private Subnet 2, linked to the LabGroup target group, with desired/minimum/maximum capacity of 2/2/6 and a target tracking scaling policy set to maintain 60% average CPU utilization.

Step 4: Verify Load Balancing
Confirm that two new "Lab Instance" EC2 instances were launched by Auto Scaling and that both show a "healthy" status in the LabGroup target group. Copy the Load Balancer's DNS name and open it in a browser to confirm the application is being served correctly through the load balancer.

Step 5: Test Auto Scaling
Lower the scaling policy's target CPU value to 50% to make scaling trigger sooner, then use the application's "Load Test" feature to generate high CPU load across the instances. Monitor the CloudWatch alarms (AlarmLow/AlarmHigh) until AlarmHigh enters the "In alarm" state, then verify in the EC2 console that additional instances were automatically launched to handle the load.

Step 6: Terminate the Original Web Server
Select Web Server 1 (the original instance used to create the AMI) and terminate it, since it is no longer needed once the Auto Scaling group is managing instances independently.

<h1>COMMANDS</h1>
No CLI commands are used in this experiment, as it is performed entirely through the AWS Management Console (GUI-based setup) using EC2, Elastic Load Balancing, Auto Scaling, and CloudWatch services.

```
HALIMA HARISHA A
212224040094
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3a3d161d-5349-46a5-9043-6c12ef4ceafa" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/43d50c8e-32a3-4d93-924d-5f6a0b730087" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/96243ad5-90f7-4bdf-8c2f-6cb078740971" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b0e28fb1-2b2a-418e-8ead-cc3a24d9053e" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e0e83f3f-ae07-4a40-abc7-c524a93177e6" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/eebe7577-bc5a-426a-9f83-450784b26217" />



<h1>RESULT</h1>
Thus, an AMI was created from a running EC2 instance, a Load Balancer was configured to distribute traffic across multiple instances, an Auto Scaling group was set up with a target tracking scaling policy, and the infrastructure was verified to automatically scale out under increased load using CloudWatch alarms.
