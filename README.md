This tutorial is dedicated to one of the many solutions to upload media files to S3 bucket.
In this tutorial, Python3, FastAPI, Docker and AWS services are being used. AWS services are created using cloudformation templates. API's are created using FastAPI library.
This repository contains the functioning code for uploading the images to the S3 bucket using python API.


First, clone the repository on your local system.

<h3>$git clone https://github.com/ragnarlegacy/S3_image_uploader.git</h3>

<h2>After cloning the repository, How to setup further?</h2>
   
1. Change to the cloudformation directory

   <b>$cd cloudformation/</b>
   
   To create stack through cloudformation templates. Execute the following commands:
   
   <b>$aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://aws_instance_creation.yaml</b> ## for instance creation
   
   <b>$aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://s3_bucket_creation.yaml</b> ## for S3 bucket creation creation
   
    To update stack configuration. Execute the following commands:
   
   <b>$aws cloudformation create-stack --stack-name <name_of_choice> --template-body file://aws_instance_creation.yaml</b> ## for instance creation
   
    To delete stack. Execute the follwoing commands:
   
   <b>$aws cloudformation create-stack --stack-name <name_of_choice></b>  ## to delete the stack
   
   Note: 
   1. Create keypair to access EC2 instance with console : <b>Create instance > scroll to keypair > Click Generate Key pair > Download the pem file</b>
   
   2. Or Using SSH : <b>ssh-keygen -t rsa -m PEM</b>

2. Connect with the EC2 instance created using the key pair.

   <b>$ssh -i "path_to_key_pair.pem" ec2-user@ec2-44-202-23-112.compute-1.amazonaws.com</b>

3. <b>$sudo yum update -y</b>               ## update the machine

   <b>$sudo yum install docker git -y</b>   ## install docker and git
   
   <b>$sudo systemctl status docker</b>     ## check docker service status
   
   <b>$sudo systemctl start docker && sudo systemctl enable docker</b> ## to start and enable docker service
   
4. Clone the repository to the EC2 instance

   <b>$git clone https://github.com/ragnarlegacy/S3_image_uploader.git</b>
   
5. Change into the directory

   <b>$cd S3_image_uploader/</b>

6. Build the Docker container image

   <b>$sudo docker build -t friendsurance:v1 .</b>
   
   <b>$sudo docker images</b> ## check with this command. Image would be there in the output friendsurance as name and v1 as tag
   
7. Deploy the container image

   <b>$sudo docker run -it -d --env-file=env.list --p 8081:80 --name friendsurance_uploader friendsurance:v1</b>
   
   #Here Name of the container is <b>friendsurance_uploader</b>, <b>environment variables</b> are passed through <b>env.list file</b>(you can even pass them at     the cli by using -e key:value), image name is <b>friendsurance:v1</b>
   
8. You can check the running status of the docker container

   <b>$sudo docker ps -a</b>
   Output will be like this:
   
   [ec2-user@ip-172-31-83-194 ~]$ <b>sudo docker ps -a</b>
   
   <b>CONTAINER ID          IMAGE             COMMAND                  CREATED          STATUS                PORTS                                   NAMES</b>
  
   <h4>629867d0c0ea     friendsurance:v1   "uvicorn main:app --…"      2 hours ago      Up 2 hours    0.0.0.0:8081->80/tcp, :::8081->80/tcp   friendsurance_container</h4>
   
9. Now finally, your api's are ready to go and can browse the api url something like this on the browser
   <h3>http://ec2_instance_public_ip_address_or_domain:8081/</h3>  ## By invoking this url, success results will returned.

10. FastAPI has inbuilt feature of Swagger to document apis'. So, invoke the url like this and playaround with the API's.
    <h3>http://ec2_instance_public_ip_address_or_domain:8081/docs</h3>


      
      
<b>Suggestions:</b> 
This task can also be achived by creating a Lambda Function written in python, behind an AWS API Gateway and with a S3 bucket attached. Will try this next.
