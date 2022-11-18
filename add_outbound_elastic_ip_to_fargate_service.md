### Launch tasks into a public subnet, with a public IP address, so that they can communicate to ECR and other backing services using an internet gateway


### Launch tasks in a private subnet that has a VPC routing table configured to route outbound traffic via a NAT gateway in a public subnet. 
This way the NAT gateway can open a connection to ECR on behalf of the task.(Recommended)
+ Create a NAT gateway with Elastic Ip and public subnet(We can use an existing one or you can create a new one if it's needed).
+ Create a routing table (Destination (0.0.0.0/0) and Target the NAT gateway that's created previous step.)
+ Create a private subnet (This will be connected to the Fargate service.) in the VPC of the Fargate service.

### Launch tasks in a private subnet and make sure you have AWS PrivateLink endpoints configured in your VPC,
for the services you need (ECR for image pull authentication, S3 for image layers, and AWS Secrets Manager for secrets).
