# AWS Cloud Technical Essentials: Creating a VPC and Launching a Web Application in an Amazon EC2 Instance

## Objectives

After completing this lab, you will be able to:

- Understand Amazon VPC configuration
- Validate Subnet and Internet Gateway infrastructure
- Confirm a Route Table has a Public Route to the Internet
- Create Security Group rules
- Launch an Amazon Elastic Compute Cloud (Amazon EC2) instance
- Configure an EC2 instance to host a web application using a user data script
### Prerequisites

This lab requires:

- Notebook computer with Wi-Fi and Microsoft Windows, macOS, or Linux (Ubuntu, SuSE, or Red Hat)
- Administrator access (Microsoft Windows users)
- Internet browser, such as Chrome, Firefox, or Edge

## Task 1: Understand a Virtual Private Cloud’s Configuration

In this task, you review an Amazon VPC, that has been created for you, and all the infrastructure attached to and within it. You look at configuration of it’s internet gateway, route table, and subnets.

Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you’ve defined. This virtual network closely resembles a traditional network that you’d operate in your own data center, with the benefits of using the scalable infrastructure of AWS.

An _Internet gateway_ is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.

After creating a VPC, you add a _subnet_ or multiple subnets. Each subnet resides entirely within one Availability Zone and cannot span multiple Availability Zones. A subnet is a range of IP addresses in your VPC. You can launch AWS resources into a specified subnet. Use a _public subnet_ for resources that must be connected to the internet, and a _private subnet_ for resources that won’t be connected to the Internet. You will not use _private subnets_ in this lab.

**Note:** You must use the same Region throughout the lab. If you have changed the region, please revert to the previous region in which you launched this lab; you can do so by referring the **Region** parameter on the left side of these instructions.

![[Pasted image 20250722102754.png]]
## Task 2: Create a VPC Security Group

In this task, you review the VPC security group pre-created to launch your web application instance. A security group controls the traffic that is allowed to reach and leave the resources that it is associated with. For example, after you associate a security group with an EC2 instance, it controls the inbound and outbound traffic for the instance, similar to a firewall.
![[Pasted image 20250722103454.png]]

## Task 3: Launch Your Amazon EC2 Instance

![[Pasted image 20250722104150.png]]![[Pasted image 20250722104523.png]]
![[Pasted image 20250722104919.png]]
![[Pasted image 20250722104934.png]]![[Pasted image 20250722105106.png]]
![[Pasted image 20250722110730.png]]
![[Pasted image 20250722110736.png]]
![[Pasted image 20250722111247.png]]

## Additional Resources

- For more information about Amazon VPCs, see [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/).
- For more information about Amazon EC2, see [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/).
- For more information about AWS Training and Certification, see [http://aws.amazon.com/training/](http://aws.amazon.com/training/).