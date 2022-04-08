# FinalUpgradAssignment
This is the final project of DevOps PG Course

Setup infrastructure(infra) using Terraform (tf)
Task1
  Subtask 1	
    1.1 Ensure AWS CLI is installed and configured with full access in your Linux machine. 
    1.2 Initialize a bucket in s3 for the backend state store using Terraform. This bucket will be used later in this project to use the state files of Terraform.

      Steps	Guidelines
          Pick region as ‘us-east-1’

  Subtask 2	
    2.1 Create the following: 
        AWS VPC,
        1 IGW,
        1 NAT-GW in AZ-a, 
        Subnets (2 public & 2 private, 1 each in AZ-a &b), 
        Route Tables for both subnets
        Choose a /16 CIDR for VPC and /24 CIDRs for other subnets

        Steps Guidelines
          Internet traffic of private subnet should route via NAT-GW
          CIDRs  of VPC & Subnet should be easily configurable
          Ensure relevant data is present as output.
        
  Subtask 3	
      In the following subtask, you will create EC2 instances (bastion, Jenkins, app) & ALB. As part of this subtask, you are required to create security groups for             them.  Create Security Groups(SGs) for the following resources: 

    3.1 Bastion host SG - Allow self ip to ssh to ‘bastion’ instance and allow all egress.
    3.2 Private Instances SG - Allow all incoming traffic from within VPC and all egress.
    3.3 Public Web SG - Allow incoming to port 80 from self IP and all egress.
 
       Steps Guidelines
      You can make use of public APIs like http://ipv4.icanhazip.com, https://api.ipify.org/
      ,etc. to get the self-IP. Try to design the script in such a way that self-IP need not be manually entered in the script. 
      
   Subtask 4	
Create a key pair that will be used by the users to access the EC2 instances.
Create 3 ec2 instances with the names - bastion, Jenkins, app- using Ubuntu-20 official AMI.
Only the ‘bastion’ instance should be directly accessible over the internet. 
SG created earlier should be used to allow the users to have ssh access to the bastion host from self ip.
Instances can be in any or both AZs
Prefer small instance types (1 vCPU may be enough)
ssh to private ec2 hosts should be possible using the Proxyjump feature.

Subtask Bonus	Document your VPC architecture diagram to showcase networking.


Task2
Subtask 1

Install ansible.
Bootstrap ansible codebase for managing Jenkins and app hosts.
Write an inventory file.
Install docker on Jenkins and app hosts using ansible.
 
Ensure that the docker service is started after docker installation is complete. 

Avoid running any container via ansible

Prefer running ansible from the local system

Subtask 2	
Create an ALB for the following requirements:

ALB listens on port 80 and should be internet-facing.
ALB forwards /jenkins, /jenkins/* to a Target Group having Jenkins host (port 8080) as backend
ALB forwards /app, /app/* to a Target Group having app host (port 8080) as backend
Make use of security groups created in Task 1
 
Subtask 3	
Manually install Jenkins on the ‘jenkins’ instance. 
Access it via ALB endpoint to configure further (Install recommended plugins when prompted).
Create another user inside Jenkins during configuration with admin access

In order to install Jenkins in the  ‘Jenkins’ instance, you need to access its terminal. You will have to access its terminal via the Bastion host you created in the public subnet. 

Try not to store the private key in the bastion host. Try using the proxy jump command. 

Jenkins uses --prefix tag to configure access to Jenkins dashboard via non-root( / ) path. Therefore, in order to access the Jenkins dashboard from ALB on URL <alb-dns>/jenkins, make suitable changes in the appropriate file.
  
  Subtask 4	
Create an ECR repository to store the docker image of the node application. This repository will be later used in this project for the deployment of the node application. Create & attach IAM role to provide ECR access to ‘jenkins’ and ‘app’ hosts.
Ensure that ‘jenkins’ and ‘app’ hosts are authenticated to use the ECR repository you created. 
Ensure a sample Jenkins job is able to ssh to the app host and run simple commands. This is needed so that Jenkins can ssh to the ‘app’ instance, to pull docker images and spin up containers.
- ‘amazon-ecr-credential-helper’ can also be used for ECR authentication. 

- ssh-keygen can be used to generate new key pairs in the EC2 instance

- Generate a new key pair for this task. Make sure your personal ssh keys ($HOME/.ssh/id_rsa) are not used/updated in this subtask. 
 Subtask Bonus	
Complete Subtask 2 (setting up of ALB and TGs) using terraform.
  
  
  Task3  
  Dockerize sample Node.js app & write Jenkinsfile
  
  Subtask 1

Create a repository  (preferably GitHub) and copy the sample app code in it. (You can download the code base for the node application from the bottom of this segment). Note that this is a simple demo node application with just 1 API defined. 
Write Dockerfile to dockerise the node application. This file should be able to install dependencies for the application and expose the application on port 8080
Test docker build and run on your local system
Push the docker file of the node application in the same GitHub repository.
Use the alpine version of the node image. 

Create a private git repository to store the node application code base and the docker file.
  
  
  Subtask 2	
Create a Jenkinsfile in the repository.

 

It should define at least 2 stages -
Stage 1: This stage should contain the steps to git checkout the repository containing the docker file and the application code. 
Stage 2: This stage should contain the steps to build a docker image of the node application and then publish the image to the ECR repository created in the previous task.
Create a pipeline job in Jenkins which pulls your git repo and uses the Jenkinsfile for building.
Hint: docker-workflow, git and ssh plugins might be needed in Jenkins


Make use of variables and parameters in the Jenkins file. 
 
  
Subtask 3	
Add another stage (deploy to app host) in Jenkinsfile which deploys the new container in the’ app’ host.

1 suggested way for this stage is: stage should ssh into app host (root user), check if older image container is running and stop it, run new image container.
 

Hint: Having a static name to the container can be helpful in checking if the container is already running. 
  
  Subtask Bonus	Calculate the total hourly/monthly cost of the complete infrastructure.	
 
 
 
 
