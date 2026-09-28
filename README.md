AWS VPC + Application Load Balancer Web Application Deployment

Project Overview

This project demonstrates the deployment of a modern static web application on AWS using Amazon EC2, Amazon VPC, Application Load Balancer (ALB), subnets, route tables, Internet Gateway, and NAT Gateway.

The application is a responsive single-page website called NEXORA — The Future Starts Here. The website is implemented in one index.html file containing HTML, CSS, and JavaScript.

The AWS environment shown in this project includes a custom VPC (test-vpc001) with CIDR 10.0.0.0/24, two subnets across us-east-1a and us-east-1b, route tables, an Internet Gateway, a NAT Gateway, EC2 instances, and an internet-facing Application Load Balancer.

Project Name

NEXORA – AWS VPC and Application Load Balancer Web Application Deployment

Architecture

                         Internet
                            |
                            |
                  Application Load Balancer
                     (Internet-Facing)
                            |
                     HTTP : 80
                            |
                     Target Group
                            |
                +-----------+-----------+
                |                       |
             EC2 Instance           EC2 Instance
             Web Server             Web Server
             us-east-1a             us-east-1b
                |                       |
                +-----------+-----------+
                            |
                    Custom AWS VPC
                     10.0.0.0/24
                            |
             +--------------+--------------+
             |                             |
         Subnet A                       Subnet B
         us-east-1a                     us-east-1b
             |                             |
        Route Table                    Route Table
             |                             |
        Internet Gateway              NAT Gateway

The screenshots show two running t3.micro EC2 instances in different Availability Zones. The ALB resource map currently shows one registered healthy target on port 80, so the diagram represents the intended deployment architecture rather than claiming that both instances were actively registered with the target group at the time of the screenshot.

AWS Resources Used

AWS Service

Purpose

Amazon VPC

Provides the isolated network environment

Subnets

Separates resources across Availability Zones

Route Tables

Controls network traffic routing

Internet Gateway

Provides internet connectivity for the public side of the VPC

NAT Gateway

Provides outbound internet connectivity for private-network resources

Amazon EC2

Hosts the web application

Application Load Balancer

Receives HTTP traffic and forwards requests to the target group

Target Group

Contains the EC2 web-server target(s)

Security Groups

Controls inbound and outbound network traffic

Amazon CloudWatch

Used for EC2 monitoring and metrics

VPC Configuration

The project uses a custom VPC:

VPC Name: test-vpc001
CIDR:     10.0.0.0/24
Region:   us-east-1

The VPC resource map shown in the AWS Console contains:

2 subnets

4 route tables

Internet Gateway

NAT Gateway

Network connections associated with the VPC

The subnets are located in:

us-east-1a
us-east-1b

EC2 Configuration

The AWS Console screenshots show running EC2 instances using:

Instance Type: t3.micro
Region:        us-east-1

The instances are distributed across:

Availability Zone 1: us-east-1a
Availability Zone 2: us-east-1b

This layout provides the foundation for distributing web-server resources across multiple Availability Zones.

The EC2 monitoring dashboard also shows metrics such as:

CPU utilization

Network In

Network Out

Network packets

CPU credit usage

CPU credit balance

Metadata token metrics

Application Load Balancer

The project uses an internet-facing Application Load Balancer.

Configuration visible in the AWS Console:

Load Balancer: test-lb001
Type:          Application Load Balancer
Scheme:        Internet-facing
IP Type:       IPv4
Listener:      HTTP : 80
Status:        Active

The ALB receives requests from users and forwards HTTP traffic to the configured target group.

The ALB DNS endpoint shown in the AWS Console follows this pattern:

test-lb001-xxxxxxxx.us-east-1.elb.amazonaws.com

The ALB resource map shows:

HTTP : 80
      |
      v
Default Rule
      |
      v
Target Group
      |
      v
EC2 Web Server
Port 80
Healthy

Web Application

The deployed website is called NEXORA.

The application is a single-page frontend containing:

HTML

CSS

JavaScript

All three are contained in a single index.html file.

The page includes:

Hero Section

BUILD
BEYOND

with animated gradient text and a typing animation.

Navigation

The website contains navigation links for:

Home

Features

Stats

Contact

Features

The application displays four feature cards:

Lightning Fast

Smart Technology

Secure by Design

Global Scale

Animated Statistics

The page includes animated counters for:

Uptime

Projects

Countries

Support

Contact / CTA

The page contains a call-to-action section:

READY TO BUILD?

with a button for starting a project.

Frontend Technologies

The website uses:

HTML5
CSS3
JavaScript

No frontend framework is required.

The index.html contains the complete frontend implementation.

Frontend Features

The website includes several CSS and JavaScript effects.

1. Animated Gradient Background

The page uses animated radial gradients to create a futuristic background.

2. Floating Particles

JavaScript dynamically creates 80 particle elements and animates them vertically across the page.

3. Typing Animation

The hero section cycles through:

beautiful.
powerful.
different.
crazy.
unforgettable.

4. Cursor Glow

A glowing effect follows the user's mouse pointer.

5. Scroll Reveal

Sections become visible with a smooth animation as the user scrolls.

6. Animated Counters

The statistics section uses JavaScript to animate numbers when the section becomes visible.

7. 3D Card Hover

Feature cards react to mouse movement using CSS 3D transforms.

8. Button Ripple Effect

Buttons display a ripple animation when clicked.

9. Responsive Design

The website includes a mobile media query so the layout adapts to smaller screens.

Suggested Repository Structure

nexora-aws-load-balancer/
│
├── index.html
├── README.md
│
└── screenshots/
    ├── ec2-monitoring.png
    ├── nexora-website.png
    ├── vpc-resource-map.png
    └── load-balancer-resource-map.png

Deployment Flow

The overall deployment flow is:

1. Create VPC
       |
2. Create Subnets
       |
3. Configure Route Tables
       |
4. Attach Internet Gateway
       |
5. Configure NAT Gateway
       |
6. Launch EC2 Web Servers
       |
7. Deploy NEXORA index.html
       |
8. Configure Web Server on Port 80
       |
9. Create Target Group
       |
10. Register EC2 Target
       |
11. Create Application Load Balancer
       |
12. Configure HTTP : 80 Listener
       |
13. Forward Requests to Target Group
       |
14. Access Application through ALB DNS

How the Request Works

When a user opens the ALB DNS name:

Browser
   |
   | HTTP Request : 80
   v
Application Load Balancer
   |
   v
Listener : HTTP 80
   |
   v
Target Group
   |
   v
EC2 Web Server
   |
   v
NEXORA index.html
   |
   v
Browser

The ALB resource map in the project shows the HTTP listener forwarding traffic to the target group and a healthy EC2 target on port 80.

Monitoring

EC2 monitoring was checked from the AWS Console.

The monitoring dashboard provides metrics including:

CPU Utilization
Network In
Network Out
Network Packets In
Network Packets Out
CPU Credit Usage
CPU Credit Balance
Metadata Token Count

These metrics can be used to understand the health and activity of the EC2 web server.

Security Considerations

For a production version of this architecture, security groups should follow the principle of least privilege.

Recommended traffic flow:

Internet
   |
   v
ALB Security Group
   |
   | HTTP/HTTPS
   v
EC2 Security Group
   |
   | Application traffic
   v
Web Server

The EC2 security group should preferably allow web traffic from the ALB security group instead of allowing unrestricted access directly from the internet.

For HTTPS production deployments, an SSL/TLS certificate can be attached to the ALB using AWS Certificate Manager (ACM).

Future Improvements

Possible improvements to this project include:

Registering both EC2 instances in the target group

Configuring health checks

Adding HTTPS with AWS Certificate Manager

Adding Route 53 DNS

Adding Auto Scaling Group

Adding CloudWatch alarms

Automating infrastructure with Terraform or CloudFormation

Creating a CI/CD pipeline with GitHub Actions

Moving static assets to Amazon S3

Using CloudFront for global content delivery

Adding a custom domain

Adding HTTPS-only redirects

What I Learned

This project provides practical exposure to:

AWS VPC networking

CIDR and subnet design

Availability Zones

Route tables

Internet Gateway

NAT Gateway

EC2 deployment

Application Load Balancer

Target Groups

HTTP listeners

Security Groups

CloudWatch/EC2 monitoring

Basic web-server deployment

Static frontend deployment

AWS architecture troubleshooting

Screenshots

Add the project screenshots to the repository under:

screenshots/

Recommended names:

screenshots/ec2-monitoring.png
screenshots/nexora-website.png
screenshots/vpc-resource-map.png
screenshots/load-balancer-resource-map.png

Then add them to this README:

## EC2 Monitoring

![EC2 Monitoring](screenshots/ec2-monitoring.png)

## NEXORA Application

![NEXORA Application](screenshots/nexora-website.png)

## VPC Resource Map

![VPC Resource Map](screenshots/vpc-resource-map.png)

## Application Load Balancer

![Application Load Balancer](screenshots/load-balancer-resource-map.png)

Project Status

Status: Successfully deployed and tested through the AWS Application Load Balancer endpoint shown in the project screenshots.

The screenshots document the AWS infrastructure and the NEXORA web application running through the ALB.

Author

AWS / DevOps Project

Built as a hands-on AWS networking and web-application deployment project.

License

This project is intended for learning and portfolio purposes.
