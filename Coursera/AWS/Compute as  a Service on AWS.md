## Understanding Servers

The first building block you need to host an application is a server. Servers often times can handle Hypertext Transfer Protocol (HTTP) requests and send responses to clients following the client-server model, though any API based communication also falls under this model. A client being a person or computer that sends a request, and a server handling the requests is a computer, or collection of computers, connected to the internet serving websites to internet users. These servers power your application by providing CPU, memory, and networking capacity to process users’ requests and transform them into responses. For context, common HTTP servers include:

- Windows options, such as Internet Information Services (IIS).
    
- Linux options, such as Apache HTTP Web Server, Nginx, and Apache Tomcat.
    

To run an HTTP server on AWS, you need to find a service that provides compute power in the AWS Management Console. You can log into the console and view the complete list of AWS compute services.

![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/AptFnK9UQl2dFBwc69YDPw_024782925ab349f483d708a75ef1c3f1_asset-v1-AWS-AWS-AWS-OTP-AWSD16-1T2023-type-asset-block-Reading_2.1_Compute_services.png?expiry=1753142400000&hmac=yKSBs95u1p4J9k1zTW1hAwUT7G-itv7bY7zsJe0yE6U)

## Choose the Right Compute Option

If you’re responsible for setting up servers on AWS to run your infrastructure, you have many compute options. You need to know which service to use for which use case. At a fundamental level, there are three types of compute options: virtual machines, container services, and serverless. If you’re coming to AWS with prior infrastructure knowledge, a virtual machine can often be the easiest compute option in AWS to understand. This is because a virtual machine emulates a physical server and allows you to install an HTTP server to run your applications. To run these virtual machines, you install a hypervisor on a host machine. This hypervisor provisions the resources to create and run your virtual machines.In AWS, these virtual machines are called Amazon Elastic Compute Cloud or Amazon EC2. Behind the scenes, AWS operates and manages the host machines and the hypervisor layer. AWS also installs the virtual machine operating system, called the guest operating system.Some AWS compute services use Amazon EC2 or use virtualization concepts under the hood, therefore it is best to understand this service first before moving on to container services and serverless compute.

**Resources**:

- [_External Site:_ AWS: Compute Services Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/compute-services.html)
    

- [_External Site:_ AWS: Compute on AWS](https://aws.amazon.com/products/compute/)
    

- [_External Site:_ AWS: AWS Compute Blog](https://aws.amazon.com/blogs/compute/)