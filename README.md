# Building-an-Amazon-VPC
aws configure
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications "ResourceType=vpc,Tags=[{Key=Name,Value=Lab VPC}]"
aws ec2 modify-vpc-attribute --vpc-id vpc-0950a6d0127f968c4 --enable-dns-support
aws ec2 modify-vpc-attribute --vpc-id vpc-0950a6d0127f968c4 --enable-dns-hostnames
aws ec2 create-subnet --vpc-id vpc-0950a6d0127f968c4 --cidr-block 10.0.0.0/24 --availability-zone us-east-1a --tag-specifications "ResourceType=subnet,Tags=[{Key=Name,Value=Public Subnet}]"
aws ec2 create-subnet --vpc-id vpc-0950a6d0127f968c4 --cidr-block 10.0.2.0/23 --availability-zone us-east-1a --tag-specifications "ResourceType=subnet,Tags=[{Key=Name,Value=Private Subnet}]"
aws ec2 modify-subnet-attribute --subnet-id subnet-0948afb12034ea524 --map-public-ip-on-launch
aws ec2 create-internet-gateway --tag-specifications "ResourceType=internet-gateway,Tags=[{Key=Name,Value=Lab IGW}]"
aws ec2 create-route-table --vpc-id vpc-0950a6d0127f968c4 --tag-specifications "ResourceType=route-table,Tags=[{Key=Name,Value=Public Route Table}]"
aws ec2 create-route --route-table-id rtb-0ddff7ef59dc3fdf4 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-05b8c2fbbefa24a52
aws ec2 associate-route-table --subnet-id subnet-0948afb12034ea524 --route-table-id rtb-0ddff7ef59dc3fdf4
aws ec2 create-security-group --group-name App-SG --description "Security group for app server" --vpc-id vpc-0950a6d0127f968c4
aws ec2 authorize-security-group-ingress --group-id sg-0abe956cf3a5e755c --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 run-instances --image-id ami-0fff1b9a61dec8a5f --count 1 --instance-type t2.micro --key-name vockey --security-group-ids sg-0abe956cf3a5e755c --subnet-id subnet-0948afb12034ea524 --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=App Server}]" --user-data '#!/bin/bash ...'
