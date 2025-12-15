# Building-a-web-app-with-EC2-and-DynamoDB
## Objective
The objective was to learn how to build and secure a cloud-based web application by connecting an EC2-hosted application to DynamoDB using IAM roles, while understanding networking, permissions, and real-world troubleshooting on AWS.

## Architecture Diagram And Overview
This project implements a simple, secure web application architecture on AWS using a compute layer hosted on Amazon EC2 and a fully managed NoSQL database provided by Amazon DynamoDB. The design follows AWS best practices by separating compute and data layers and using IAM roles for secure service-to-service communication.


![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(210).png)

  Image 01: Architecture Diagram

##  Phase Invovle In Building oF This Project  
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


  Creating a Route Table for the Public Subnet , After creating the route table , I added a route with :
 
   -Destination: 0.0.0.0/0 ( The internet)
   
   -Target : IGW 
   
  As shown in the image below :
  
  ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(131).png)

   Image 07.

Than, I have to Associate the route table with the Public Subnet in the page shown in the image below :

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(133).png)

   Image 08.


### Phase 2 : Create DynamoDB Table 
In the creation of the DB , the following settings  and configuration were used :
  - Table name: Users
  - Partition key: userid(String)
as shown in the image below :

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(145).png)


Image 09.


Launching an EC2 in the public subnet in the VPC we created , The specs of the EC2 are as follows :

- OS -> Ubuntu

- Instance type -> t3.micro

- Public IP -> Enable

- Security Group -> allow ssh from anywhere .
                 -> allow https/http from anywhere
   note: the reason for shh from anywhere is because of my IP address changes as I join one network to another.

  ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(164).png)

   image 10: updating the EC2

   ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(166).png)

   image 10: upgrading the EC2

Creating IAM Role For EC2  : This is to important because without this ,the server will not be able to read or write into the DB. In order to do this , I went to IAM -> attach permission-> create policy , to create a policy to allow EC2
to read and write in the DB, as shown in the image below :

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(153).png)


Image 11: The policy in JSON .


![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(154).png)


Image 12: review of the policy , but the name of the policy is not in the image .

After this , i went to create the role ; IAM-> Roles -> Create role 
- Trusted entity : AWS service
- Use case : EC2, , as shown below:

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(156).png)

Image 13.

Attaching the pocily create before to the role 

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(157).png)

 Image 14.


 ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(158).png)

Image 15: review of the role be create.



  
