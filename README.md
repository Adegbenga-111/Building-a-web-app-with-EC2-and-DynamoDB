# Web Application Deployment with Amazon EC2 and DynamoDB

## Project Summary

This project demonstrates how to design, deploy, and secure a simple cloud-based web application on AWS using **Amazon EC2** for compute and **Amazon DynamoDB** as a fully managed NoSQL database. The focus is on **secure service-to-service communication** using IAM roles, proper network configuration, and real-world troubleshooting.

The application runs on an EC2 instance and performs read/write operations to DynamoDB without hard-coded credentials, following AWS security best practices.

---

## Architecture Overview

The architecture separates the compute and data layers, with EC2 hosting the application logic and DynamoDB handling persistent storage. IAM roles are used to grant the EC2 instance least-privilege access to the database.

![Architecture Diagram](https://github.com/Adegbenga-111/Building-a-web-app-with-EC2-and-DynamoDB/blob/main/Screenshot%20\(210\).png)

**Key Components:**

* Amazon VPC (custom networking)
* Public subnet with Internet Gateway
* Amazon EC2 (PHP web application)
* Amazon DynamoDB (NoSQL database)
* IAM role and policy for secure access

---

## Skills Demonstrated

* AWS VPC and subnet configuration
* EC2 deployment and Linux server management
* IAM roles and least-privilege access control
* DynamoDB table design and access patterns
* Secure application-to-database integration
* Real-world troubleshooting and debugging

---

## Implementation Phases

### Phase 1: Network and Compute Setup

* Created a custom VPC (`10.0.0.0/16`) with a public subnet (`10.0.1.0/24`)
* Attached an Internet Gateway and configured route tables for internet access
* Launched an Ubuntu EC2 instance with HTTP/HTTPS and SSH access

---

### Phase 2: DynamoDB Configuration

* Created a DynamoDB table named **Users**
* Configured the partition key as `userid` (String)
* Designed the table to support simple write and read operations from the application

---

### Phase 3: IAM Role and Permissions

* Created a custom IAM policy granting read/write access to the DynamoDB table
* Attached the policy to an EC2 IAM role
* Assigned the role to the EC2 instance to eliminate the need for hard-coded AWS credentials

---

### Phase 4: Application Setup on EC2

* Installed and configured Apache, PHP, and required PHP extensions
* Installed Composer and the AWS SDK for PHP
* Fixed file permissions to allow Apache to serve application files
* Developed a simple PHP application (`index.php` and `save.php`) to interact with DynamoDB

---

## Testing and Validation

* Accessed the application via the EC2 public IP
* Verified successful data submission and storage in DynamoDB
* Confirmed that database access worked exclusively through the EC2 IAM role

---

## Incident and Troubleshooting (Unrecorded)

During development, the application initially failed to write data to DynamoDB. Although screenshots were not captured, the root causes were identified and resolved:

1. **Region Mismatch**

   * The AWS SDK in `save.php` was configured for a different region than the DynamoDB table
   * Fix: Updated the SDK configuration to match the DynamoDB region

2. **Partition Key Case Mismatch**

   * DynamoDB partition key was defined as `userid`, while the application attempted to write `Userid`
   * Fix: Updated the application code to match the exact partition key name (case-sensitive)

This incident reinforced the importance of region consistency, schema accuracy, and careful SDK configuration when integrating AWS services.

---

## Conclusion

This project demonstrates a secure and practical approach to building a cloud-based web application on AWS using EC2 and DynamoDB. By leveraging IAM roles, the solution avoids hard-coded credentials and follows least-privilege security principles.

Through hands-on implementation and troubleshooting, I gained experience in AWS networking, IAM configuration, DynamoDB access patterns, and application-level debugging. The architecture is intentionally extensible and can be expanded with load balancers, auto scaling, containerization, or CI/CD pipelines to support production-grade workloads.

This project serves as a strong example of my progression from cloud support fundamentals toward junior cloud engineering responsibilities.
