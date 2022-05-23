IdentityE2E

## Deploying to AWS
1. Create roles within IAM
    - These will need to be for CodeDeploy and EC2CodeDeploy.
2. Create an EC2 instances with Amazon Linux as the AMI. 
3. Within the "Configure Instance" section, select the IAM role as EC2Code Deploy.
4. Under "User Data", paste the below information: 
*#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
cd /home/ec2-user
wget https://aws-codedeploy-eu-west-2.s3.eu-west-2.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto
sudo yum install -y python-pip
sudo pip install awscli*
5. Under "Configure Security Groups", add another type for "HTTP" on port 80 and "SSH" on port 22.
6. Add tags to the instances, Key: "Name" and Value: "CodeDeploy".
7. Launch the instance and ensure that it's all working.
8. Go to the CodeDeploy section and select "Create Application". 
9. Create a deployment Group and choose a name for the new group.
10. Under the Service Role, select the role that was created earlier. 
11. Under "Environment Configuration", choose "Amazon EC2 instances" and select the instance that was create earlier.
12. Unticket "Enable load balancing".
13. On the main screen, select "Create Deployment".
14. Select the deployment group that was recently created. 
15. Select "My application is stored in GitHub" - this will require you to sign into GitHub.
16. Enter the Repo name - this will need to be your username and then the repo (DrogrinHunter/IndentityE2E).
17. In GitHub, find the latest commit ID and enter this into AWS.
18. This should deploy your site and you will now be hosting your GitHub repo.

## DynamoDB
1. Under AWS, search for DynamoDB.
2. Select "Create Table", enter the Table Name - for this example I have called it UserCheckin.
3. Enter in the partition key, this can be either a string or number - this will be your identifier. 
4. Accept all of the default options.

### Considerations
- The client intends to migrate more applications in the future. How will the solution remain intuitive to new developers and scale to their needs? 
    - When creating instances, you have the ability to create more than one with using the same service roles. This allows for different developers to be able get up to speed quicker. 
- Encrypting resources at rest and in transit will help the client improve their security position. 
    - With AWS Instances, there is the ability to enter tags which reduces the risk of others getting a hold of the information. You can also change the security groups to ensure that it's locked down to different IP addresses and ports.
- How can the application be deployed in such a way to scale with demand?  
    - AWS has the functionality to change the different tiers so should developers or customers run out of space, you can edit the instance to increase the tiers.