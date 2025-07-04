# aws-capstone1
# AWS Capstone Project (Detailed)

 This document outlines a complete AWS Capstone Project involving the deployment of a full-stack Python Flask application with MySQL, S3, and DynamoDB integration, using a custom VPC, EC2 instances, IAM roles, 
 and Application Load Balancer (ALB).

# Tools & Services Used

 1.Amazon VPC

 2.Subnets (Public & Private)

 3.Internet Gateway (IGW), NAT Gateway

 4.EC2 Instances (Public & Private)

 5.RDS (MySQL)

 6.Amazon S3

 7.Amazon DynamoDB

 8.IAM Roles & Policies

 9.Application Load Balancer

 10.Flask, Python3, Boto3, PyMySQL

# Step-by-Step Implementation

1. VPC & Subnet Configuration

   VPC Name: my-vpc-proj-ajith

   IPv4 CIDR: 10.10.0.0/16

Subnets:

  Public Subnet 1:

    Name: my-pub1-ajith

    AZ: ap-south-1a

    CIDR: 10.10.0.0/24

    Auto-assign Public IP: Enabled

Public Subnet 2:

    Name: my-pub2-ajith

    AZ: ap-south-1b

    CIDR: 10.10.1.0/24

    Auto-assign Public IP: Enabled

Private Subnet:

    Name: my-priv-ajith

    AZ: ap-south-1a

    CIDR: 10.10.2.0/24

2. Internet Gateway & Routing

     IGW: my-proj-igw attached to the VPC

     Public Route Table: my-pub-ajith

     Route: 0.0.0.0/0 → IGW

     Associated with both public subnets

3. EC2 Instance Creation

     Public EC2:

         Name: myec2-pub

         OS: Ubuntu

         Subnet: my-pub2-ajith

         Public IP: Enabled

    Private EC2:

        Name: myapp-ajith

        OS: Ubuntu

        Subnet: my-priv-ajith

        Public IP: Disabled

4. Connect to Private EC2 via Bastion Host

      On myec2-pub, added private key file (key.pem)

# Set permissions:

                   sudo chmod 400 key.pem
                   ssh -i key.pem ubuntu@<private-ip>

5. Enable Internet on Private EC2

              NAT Gateway: myprojnat-ajith

              subnet: my-pub2-ajith

              Allocated EIP
 
              Private Route Table: mypriv-ajith

              Route: 0.0.0.0/0 → NAT Gateway

              Associated with my-priv-ajith

6. Software Installation (on myapp-ajith)

              sudo apt update
              sudo apt install python3 -y
              sudo apt install python3-flask -y
              sudo apt install python3-boto3 -y
              sudo apt install python3-pymysql -y
              sudo apt install mysql-server -y

7. Application Setup

Cloned repo:

                git clone https://github.com/Training-Demo/aws-code-main
                cd aws-code-main

Updated config.py with:

              customhost: RDS endpoint

              customuser: admin

              custompass: admin1234

              customdb: database name

              custombucket: S3 bucket name

              customregion: ap-south-1

8. RDS Configuration

             Engine: MySQL

             Template: Free tier

             VPC: my-vpc-proj-ajith

             Credentials: admin / admin1234

# Connected using:

              mysql -h <rds-endpoint> -u admin -p
              use <db-name>;
              show tables;

9. DynamoDB

            Table: employee

            Partition key: emp-id

10. S3 Bucket

           Name: my-proj-bucket-ajith

           Region: ap-south-1

11. IAM Role & Policy

            Created Role: myprojrole-ajith

            Attached Policies:AmazonS3FullAccess, AmazonRDSFullAccess


Attached Role to EC2 (myapp-ajith)

12. Load Balancer Setup

           Target Group: myproTG-ajith

           Type: Instance

           Port: 80

           Protocol: HTTP

           ALB: myprojALB-ajith

           Scheme: Internet-facing

          Listener: HTTP, Port 80

          AZs: ap-south-1a, ap-south-1b

          Target Group: myproTG-ajith

13. Accessing the Web App

         Open ALB DNS in browser

Input employee details:

        ID, First Name, Last Name, Skills, Location

        Upload image file

# On clicking Update Database:

       Data saved to RDS

       Image uploaded to S3

       DynamoDB stores mapping of emp-id to image URL

# Outcome

A production-style, secure, and scalable infrastructure was created using AWS services. The Flask app supports employee data and image uploads stored across RDS, S3, and DynamoDB, served through an ALB.
