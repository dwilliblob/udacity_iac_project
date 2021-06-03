<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Thanks again! Now go create something AMAZING! :D
***
***
***
*** To avoid retyping too much info. Do a search and replace for the following:
*** dwilliblob, udacity_iac_project, twitter_handle, lee.williams@evesfield.co.uk, Infrastructure as Code, Udacity project to build infrastructure as code using AWS Cloudfront
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->


<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/dwilliblob/udacity_iac_project">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Infrastructure as Code</h3>

  <p align="center">
    Udacity project to build infrastructure as code using AWS Cloudfront
    <br />
    <a href="https://github.com/dwilliblob/udacity_iac_project"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/dwilliblob/udacity_iac_project">View Demo</a>
    ·
    <a href="https://github.com/dwilliblob/udacity_iac_project/issues">Report Bug</a>
    ·
    <a href="https://github.com/dwilliblob/udacity_iac_project/issues">Request Feature</a>
  </p>
</p>



<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary><h2 style="display: inline-block">Table of Contents</h2></summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

[![Product Name Screen Shot][product-screenshot]](https://example.com)

My Udacity project demonstrating how to build infrastructure as code on AWS using Cloudformation.

### Assumptions
Requirement of this project is that application optimsed for high availability. Therefore I have decided to deploy across two availability zones rather than optimising for cost and deploying in a single availbility zone.

10.0.0.0/16 (256 x 256 = 65,000 available ip addresses)
10.0.1.0/24 (255 available addresses)
VPC provide private IP addresses
Subnets smaller subsets of available IP address space
Create subnets with expansion in mind

### Resources (and explanation)
* [Internet Gateway] - Provides access to users (testers) from the internet
* [VPC Nat Service Gateway (1 & 2)] - Provides outbound internet access to resources in private subnets - translates incoming public traffic into private traffic - requires public access (locate in public subnet)
* [Web Server (1 & 2)] - Manages Traffic from internet
* [Autoscaling Group] - Enables high availability by scaling resources to meet demand (requires 2+ subnets)
* [Load Balancer] - Provides a single entry point handling requests and distributing them equally across target group of servers providing common service
* [EC2 Server] - 
* [Security Group] - Manages inbound and outbound traffic for server instances
* [Route Table] - Rules for routing traffic to specified address ranges (IPs)
* [S3 Bucket] - AWS service outside VPC for storing assets and log files

### Built With

* [](YAML files)
* [](JSON files)
* [](SHELL Scripts)
* [](CLI Commands)

### Stacks

* VPC Stack
* Network Stack
* App Stack

<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running follow these simple steps.

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.
* npm
  ```sh
  npm install npm@latest -g
  ```

### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/dwilliblob/udacity_iac_project.git
   ```
2. Install NPM packages
   ```sh
   npm install
   ```



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_



<!-- CONTACT -->
## Contact

David Lee Williams - lee.williams@evesfield.co.uk

Project Link: [https://github.com/dwilliblob/udacity_iac_project](https://github.com/dwilliblob/udacity_iac_project)