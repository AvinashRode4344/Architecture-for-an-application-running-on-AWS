
# Architecture for an application running on AWS

Here's an architecture for an application running on AWS with EC2 instances running different containers for dev and prod, and using an Nginx reverse proxy for accessing with different URLs and subdomains:

1. Create an Elastic Load Balancer (ELB)
2. Create EC2 instances
3. Configure your Docker containers to listen on different ports
4. Install and configure Nginx on each EC2 instances
5. Configure the Nginx reverse proxy to use SSL/TLS for secure communication
6. Set up DNS records for each subdomain and URL that you want to use
7. Set up monitoring and logging for your application and infrastructure

Overall, this architecture should provide a stable environment for running your application on AWS, with separate dev and prod instances, and URLs and subdomains for easy management and testing.
## Architecture

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


## Description
### 1. Create an Elastic Load Balancer (ELB)
An Elastic Load Balancer (ELB) is a service provided by AWS that distributes incoming traffic across multiple instances. By placing an ELB in front of your EC2 instances running your application, the ELB will receive all incoming traffic to your application. It will then distribute this traffic evenly across all the instances you have registered with the ELB, ensuring that none of the instances get overwhelmed with traffic.

In addition to load balancing traffic across multiple instances, you can also configure the ELB to use a routing policy to direct traffic to different target groups based on the path of the URL or subdomain. This allows you to route traffic to different instances based on specific criteria. For example, you could direct traffic for requests to "www.example.com/dev" to the EC2 instance running your development environment, and traffic for requests to "www.example.com/prod" to the instance running your production environment.

By using an ELB with routing policies, you can ensure that incoming traffic to your application is distributed evenly and directed to the correct target group based on your specific routing requirements. This can improve the performance and reliability of your application, while also providing flexibility to handle different types of traffic.

### 2. Create EC2 instances
Creating two EC2 instances, one for dev and one for prod, means that you will have separate environments to run your application code for development and production. Each EC2 instance will run a Docker container that contains your application code. By using Docker, you can ensure that your application is running in an isolated environment that is consistent and repeatable.

To ensure that you can easily distinguish between the dev and prod environments, you should tag each Docker container with a unique name, such as "dev" and "prod". This will help you to manage your containers and keep track of which environment each container is running in. For example, you may want to deploy updates to your dev environment first, and then promote the updated code to production once you have tested it.

By launching separate EC2 instances with Docker containers for dev and prod, you can ensure that changes made to your development environment do not affect your production environment. This can help you to test and validate changes before deploying them to production, reducing the risk of introducing errors or downtime.

### 3. Configure your Docker containers to listen on different ports
To run multiple Docker containers on the same EC2 instance, you need to configure each container to listen on a different port. This is because only one process can listen on a given port at a time.

For example, you might configure your "dev" container to listen on port 8000, and your "prod" container to listen on port 9000. When a request comes in to the EC2 instance, the request will be routed to the appropriate container based on the port it is listening on.

By using different ports for dev and prod containers, you can ensure that they do not interfere with each other and that requests are routed correctly. This can help you to keep your development and production environments separate and avoid any issues that might arise from running multiple containers on the same EC2 instance.

### 4. Install and configure Nginx on each EC2 instances
Install and configure Nginx on each EC2 instance. Nginx will act as a reverse proxy, routing traffic to the correct container based on the path of the URL or subdomain. For example, requests to "www.example.com/dev" will be routed to the "dev" container running on port 8000, and requests to "dev.example.com" will be routed to the same container.

### 5. Configure the Nginx reverse proxy to use SSL/TLS for secure communication

When setting up a reverse proxy like Nginx, it is important to use SSL/TLS to ensure secure communication between clients and servers. SSL/TLS is a cryptographic protocol that encrypts data to prevent eavesdropping and tampering.

To enable SSL/TLS on the Nginx reverse proxy, you need to obtain a SSL/TLS certificate. AWS Certificate Manager (ACM) is a service that makes it easy to provision, manage, and deploy SSL/TLS certificates for use with AWS services.

You can obtain SSL/TLS certificates using ACM and configure Nginx to use the certificates. This involves configuring Nginx to listen on port 443 (the default port for HTTPS), specifying the SSL/TLS certificate file and key, and enabling SSL/TLS encryption.

Once SSL/TLS is enabled, all communication between clients and servers will be encrypted, providing an additional layer of security for your application.

### 6. Set up DNS records for each subdomain and URL that you want to use
When you set up subdomains or custom URLs for your application, you need to create DNS records to map them to the correct IP addresses or endpoints.

For example, if you want to use "www.example.com/dev" and "dev.example.com" to access your dev environment, you need to create DNS records that point to the Elastic Load Balancer (ELB) endpoint for the dev target group.

To do this, you can create a CNAME record for "www.example.com/dev" that points to the ELB endpoint for the dev target group. Similarly, you can create a CNAME record for "dev.example.com" that points to the same endpoint. This will ensure that requests to these subdomains are routed to the correct target group.

By setting up DNS records for your subdomains and URLs, you can make it easier for users to access your application and ensure that requests are routed correctly.

### 7. Set up monitoring and logging for your application and infrastructure
Monitoring and logging are essential components of any application architecture, as they help you keep track of the health and performance of your infrastructure and application.

AWS CloudWatch is a monitoring and logging service provided by Amazon Web Services that allows you to monitor your EC2 instances, ELB, and other AWS resources in real-time, and collect and analyze logs and metrics from your applications. With CloudWatch, you can set up alarms to notify you when certain metrics or events occur, monitor system and application logs, and troubleshoot issues that might arise.

To set up monitoring and logging for your application, you can use CloudWatch to:

Collect and analyze logs from your EC2 instances and containers: You can use CloudWatch Logs to collect and analyze log data from your application and infrastructure, and search and filter the logs to identify and troubleshoot issues.

Monitor system and application metrics: CloudWatch Metrics allows you to collect and track metrics such as CPU usage, memory utilization, and network traffic, and set up alarms to notify you when certain thresholds are reached.

Monitor ELB performance: You can use CloudWatch to monitor the performance of your Elastic Load Balancer (ELB), track key metrics such as request count and latency, and set up alarms to alert you when issues arise.

By setting up monitoring and logging for your application, you can proactively identify and resolve issues, optimize performance, and ensure the stability and reliability of your infrastructure.
## Summary
The architecture described above is designed to provide a stable and reliable environment for running an application on AWS. By using separate EC2 instances for development and production environments, the application can be tested and deployed without affecting the stability of the live environment.

The use of Docker containers for hosting the application code allows for easy portability and scalability, as containers can be easily replicated and scaled as required. Configuring the containers to listen on different ports for development and production also ensures that the environments are kept separate.

Using an Nginx reverse proxy provides a layer of abstraction between the application and the internet, allowing for more granular control over incoming traffic. The SSL/TLS configuration also ensures secure communication between the application and users.

By setting up separate URLs and subdomains for development and production, developers can easily manage and test the application in a controlled environment before deploying to the live production environment. Finally, monitoring and logging using AWS CloudWatch allows for detailed analysis and troubleshooting of any issues that may arise, ensuring the stability and reliability of the application.