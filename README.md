# Building-a-web-app-with-EC2-and-DynamoDB
## Objective
The objective was to learn how to build and secure a cloud-based web application by connecting an EC2-hosted application to DynamoDB using IAM roles, while understanding networking, permissions, and real-world troubleshooting on AWS.

## Architecture Diagram And Overview
This project implements a simple, secure web application architecture on AWS using a compute layer hosted on Amazon EC2 and a fully managed NoSQL database provided by Amazon DynamoDB. The design follows AWS best practices by separating compute and data layers and using IAM roles for secure service-to-service communication.


![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(210).png)

  Image 01: Architecture Diagram

##  Phase Invovle In Building oF This Project  
### Phase 1:

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

- Security Group
                    -> allow ssh from anywhere .
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

### Phase 3 : Set up of the EC2 to host the front and backend of the simple web app.
 Installing and enbling  apache2 and php with the set of commmand below :
     "sudo apt update -y
      sudo apt install apache2 php libapache2-mod-php php-cli unzip -y
      sudo systemctl start apache2
      sudo systemctl enable apache2"


 ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(167).png)

  Image 16: "sudo apt install apache2 php libapache2-mod-php php-cli unzip -y" running on the terminal.

After in installing the sever , I had to check if the server is well installed , the result is shown below :

 ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(169).png)

   Image 17: error page showing that something is not right, at this the time only ssh and https was allowed in the security Group so to fix it I updated the SG to allow http as  shown below:

 ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(173).png)

 Image 17 .

 After this I try to reach the my server agian , the result is shown below :

   ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(175).png)

Image 18.

The next step is to install Composer (a php dependency manager that automates installing and managing libraries for your project.) , using this command  
   "cd /var/www/html
    sudo apt install curl -y
    sudo curl -sS https://getcomposer.org/installer | sudo php"

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(176).png)

 Image 19.

  Installing  AWS SDK for PHP wusing " sudo php composer.phar require aws/aws-sdk-php" commamd and the result is shown below :



![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(177).png)
  
Image 20.
 This is a common error , telling that php extensions are missing since php does not install composer php extension by default .
 
 ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(175).png)

Image 21.

To fix the error , i used "sudo apt install php-cli php-mbstring php-zip php-curl php-xml -y" to install all of the extensions , as shown below :

  ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(178).png)

  Image 22.

After the extension has be  installed , i try to install AWS SDK for php , as shown below :

   ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(180).png)

   Image 23 : SDK successfully installed .
   
Fix permissions so Apache can read files with the following command :

"
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html"

The next step is to create the web file which are the index.php and save .php file in nano file editor , then restart the server :

  ![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(185).png) 
   
   
   Image 24.


###  Testing and dedugging 

To test the infrastructure , i ran "19.169.51.121/index.php" in my bowser , and it worked at frist , as shown below :

![Alt aws](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20(186).png)



### Conclusion

This project successfully demonstrated how to design and deploy a secure, cloud-based web application on AWS using Amazon EC2 and Amazon DynamoDB. By integrating a PHP web application running on EC2 with DynamoDB through the AWS SDK and IAM roles, the project showed how application components can communicate securely without the use of hard-coded credentials.

Throughout the implementation, real-world challenges such as IAM role configuration, region mismatches, DynamoDB partition key requirements, and SDK setup were encountered and resolved. These challenges reinforced practical troubleshooting skills and deepened understanding of AWS service interactions, security best practices, and cloud architecture fundamentals.

The final solution highlights the benefits of using managed services like DynamoDB for scalability and availability while maintaining a simple and flexible application layer on EC2. The architecture is intentionally designed to be extensible, providing a strong foundation for future enhancements such as load balancing, auto scaling, containerization, and hybrid or multi-cloud deployments.

Overall, this project achieved its learning objectives and serves as a solid stepping stone toward building more complex, production-grade cloud infrastructures.
   
  
