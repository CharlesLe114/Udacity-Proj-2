# Deploy a High-Availability Web App using CloudFormation

Dummy Application deployed on based network infrastructure (defined in network.yml). 

The application is accessible via load balancer port 80, no public access to Instance.

## Diagram
![Blank diagram](https://user-images.githubusercontent.com/56160926/160891127-116e7b7c-78ef-4572-8200-bd444a304904.png)

## File structure
### network.yml
  - Define base network, including:
    - VPC
    - Internet Gateway
    - 2 Public Subnet
    - 2 Private Subnet
    - 2 NAT Gateway 
    - 2 Elastic IP for each NAT gateway
    - Public Route Table, route to IGW
    - Private Route Table, route to NAT gateway
    
### servers.yml
  - Define server which host the web application, including:
    - Web Server Security Group with port 80 open for ingress
    - Launch Configuration
    - Auto Scaling Group
    - Target Group to port 80 on ASG
    - Load Balancer to access the application
    - LB Security Group with ingress from port 80 to 80
    - Listener on port 80

### create.sh, delete.sh, update.sh
  - Perform create, delete, update stack action respectively
  - Usage:
    - ./create.sh <stack_name> <template.yml> <parameters.json>
    - ./update.sh <stack_name> <template.yml> <parameters.json>
    - ./delete.sh <stack_name>

## The Basics

### Parameters
<img width="608" alt="image" src="https://user-images.githubusercontent.com/56160926/160774138-29e18f46-96b6-4aa4-974c-6881996b83c0.png">

### Resources
<img width="203" alt="image" src="https://user-images.githubusercontent.com/56160926/160774335-636039a1-c586-43af-b2d5-70396bec266c.png">

### Outputs
http://webse-WebAp-3O95HDNQHGBZ-1907323451.us-east-1.elb.amazonaws.com

<img width="325" alt="image" src="https://user-images.githubusercontent.com/56160926/160774406-0ad5d606-2acb-4e62-abc5-f74a8a8056cd.png">

### Working Test
<img width="1347" alt="image" src="https://user-images.githubusercontent.com/56160926/160893289-e26e3133-29aa-429c-8331-fbeda37d4821.png">

## Load Balancer

### Target Group
<img width="395" alt="image" src="https://user-images.githubusercontent.com/56160926/160775104-ca96c96b-a0f5-4a04-a3d1-167886feae43.png">

- The auto-scaling group needs to have a property that associates it with a target group
<img width="321" alt="image" src="https://user-images.githubusercontent.com/56160926/160775255-d61408ba-cc9b-455c-9193-e310ce95040b.png">
- The Load Balancer will have a Listener rule associated with the same target group
<img width="324" alt="image" src="https://user-images.githubusercontent.com/56160926/160775494-ece030b1-0e31-43c8-9ad1-5167716e8ecb.png">

### Health Check and Listener

- Port 80 should be used in Security groups, health checks and listeners associated with the load balancer
<img width="347" alt="image" src="https://user-images.githubusercontent.com/56160926/160777632-24e847ce-3131-4ea7-af37-56cc34bc0a5c.png">
<img width="267" alt="image" src="https://user-images.githubusercontent.com/56160926/160777717-8dd2790f-38eb-44fa-930d-09b55cb7d4ee.png">
<img width="308" alt="image" src="https://user-images.githubusercontent.com/56160926/160777811-36e674a2-656c-46d4-87f3-adfbf2ec4463.png">

## Auto-Scaling

### Subnets
- PRIV-NET ( private subnets ) for auto-scaling instances
<img width="269" alt="image" src="https://user-images.githubusercontent.com/56160926/160777970-f9836df0-6add-488c-854d-faa2e39887a6.png">

### Machine Specs
- The machine should have 10 GB or more of disk and should be a t3.small or better.
<img width="1090" alt="image" src="https://user-images.githubusercontent.com/56160926/160778702-f436075b-8857-4d3b-8262-86fde05dfcf7.png">

### SSH Key
<img width="642" alt="image" src="https://user-images.githubusercontent.com/56160926/160778899-79cd3f48-1631-4b1b-bcb4-a0fe2f241412.png">


