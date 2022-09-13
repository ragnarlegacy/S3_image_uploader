This tutorial is dedicated to one of the many solutions to upload media files to S3 bucket.
In this tutorial, Python3, FastAPI, Docker and AWS services are being used. AWS services are created using cloudformation templates. API's are created using FastAPI library.

This repository contains the functioning code for uploading the images to the S3 bucket using python API. 


First, clone the repository on your local system.
$git clone https://github.com/ragnarlegacy/S3_image_uploader.git

After cloning the repository, How to setup further?
   
1. Change to the cloudformation directory
   $cd cloudformation/
   
   To create stack through cloudformation templates:
   Execute commands:
   $aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://aws_instance_creation.yaml ## for instance creation
   $aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://s3_bucket_creation.yaml ## for S3 bucket creation creation
   
   To update stack configuration:
   Execute commands:
   $aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://aws_instance_creation.yaml ## for instance creation
   
   To delete stack:
   $aws cloudformation create-stack --stack-name <name_of_choice>  ## to delete the stack
   
   Note: 
   1. Create keypair to access EC2 instance with console : Create instance > scroll to keypair > Click Generate Key pair > Download the pem file
   2. Or Using SSH : ssh-keygen -t rsa -m PEM

2. Connect with the EC2 instance created using the key pair.
   $ssh -i "path_to_key_pair.pem" ec2-user@ec2-44-202-23-112.compute-1.amazonaws.com

3. $sudo yum update -y               ## update the machine
   $sudo yum install docker git -y   ## install docker and git
   $sudo systemctl status docker     ## check docker service status
   $sudo systemctl start docker && sudo systemctl enable docker ## to start and enable docker service
   
4. Clone the repository to the EC2 instance
   $git clone https://github.com/ragnarlegacy/S3_image_uploader.git
   
5. Change into the directory
   $cd S3_image_uploader/

6. Build the Docker container image
   $sudo docker build -t friendsurance:v1 .
   $sudo docker images ## check with this command. Image would be there in the output friendsurance as name and v1 as tag
   
7. Deploy the container image
   $sudo docker run -it -d --env-file=env.list --p 8081:80 --name friendsurance_uploader friendsurance:v1   ## Here Name of the conatiner is friendsurance_uploader, environment variables are passed through env.list file(you can even pass them at the cli by using -e key:value), image name is friendsurance:v1
   
8. You can check the running status of the docker container
   $sudo docker ps -a
   Output will be like this:
   [ec2-user@ip-172-31-83-194 ~]$ sudo docker ps -a
   CONTAINER ID   IMAGE              COMMAND                  CREATED       STATUS                   PORTS                                   NAMES
   629867d0c0ea   friendsurance:v1   "uvicorn main:app --…"   2 hours ago   Up 2 hours               0.0.0.0:8081->80/tcp, :::8081->80/tcp   friendsurance_container
   4ddc859fddc6   83dc381a6fea       "/bin/sh -c 'pip ins…"   2 hours ago   Exited (1) 2 hours ago                                           keen_lederberg
   
9. Now finally, your api's are ready to go and can browse the api url something like this on the browser
   http://ec2_instance_public_ip_address_or_domain:8081/  ## By invoking this url, success results will returned.

10. Swagger is inbuilt feature of FastAPI. So, invoke the url like this and playaround with the API's.
   http://ec2_instance_public_ip_address_or_domain:8081/docs


Suggestions: 
This task can also be achived by creating a Lambda Function written in python, behind an AWS API Gateway and with a S3 bucket attached. 
