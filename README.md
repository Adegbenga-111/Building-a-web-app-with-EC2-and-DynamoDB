# Building-a-web-app-with-EC2-and-DynamoDB
## Objective
The objective was to learn how to build and secure a cloud-based web application by connecting an EC2-hosted application to DynamoDB using IAM roles, while understanding networking, permissions, and real-world troubleshooting on AWS.

## Architecture Diagram And Overview
This project implements a simple, secure web application architecture on AWS using a compute layer hosted on Amazon EC2 and a fully managed NoSQL database provided by Amazon DynamoDB. The design follows AWS best practices by separating compute and data layers and using IAM roles for secure service-to-service communication.


![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(210).png)

  Image 01: Architecture Diagram

## Steps To Build 
### Step 1:

Create The VPC with:

-name: my-vpc-01 

-IPv4 CIDR : 10.0.0.0/16 , as shown in the image below :

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(123).png)
   
   Image 02: The reminding Setting were left on default .

Create a public Subnet in the VPC with :

-name: Public-Subnet

-CIDR : 10.0.1.0/24 .

The summary of the configure in the subnet is shown in the Image below:
  ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(126).png)
  


Creating and Attaching an Internet Gateway (IGW)  with 

- name:My-IGW
  
and the IGW is attached to the VPC created at the start of this project. The reason why IGW is important in this setup ,is because it is the DOOR that connent's the VPC to the internet.    
![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(129).png)
    
   Image 05: Creation of the IGW.
   
![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(131).png)

  Image 06: Attaching the IGW to the VPC.
