# FortiGate and FortiTester NGFW Performance Test 


## Table of Contents
  - [Overview](./README.md#overview)
  - [Deployment](./README.md#deployment)


## Overview
The purpose of this lab is for users to become more familiar with FortiGates and how to run synthetic performance tests with FortiTester to understand  performance from a capacity and sizing perspective.

During the lab, FortiTester will be setup to run HTTP and HTTPS Connection Per Second (CPS) tests through a DUT FortiGate to 4x nginx web servers behind it.  The FortiGate is setup with firewall policies to test different NGFW use cases.  Also there are preset FTS performance test exports that are provided in the outputs of the CloudFormation templates.  Finally there are affinity-interrupt FortiGate snippets for optimizing throughput tests for c5/c5n/c6i.4xlarge and bigger instance types.

For further information on FortiOS affinity settings and FortiTester in general, please visit [Fortinet Documentation site](https://docs.fortinet.com).


## Deployment
Before attempting to create a stack with the template, a few prerequisites should be checked to ensure a successful deployment:
1.	**To be able to test all supported instance types, you will need a FortiTester VM16 and FortiGate VMULv license.**
2.	An AMI subscription must be active for Amazon Linux 2, FortiGate BYOL, and FortiTester BYOL listings being used in the template.
  * [Amazon Linux 2 Marketplace Listing](https://aws.amazon.com/marketplace/pp/prodview-zc4x2k7vt6rpu)
  * [FortiGate BYOL Marketplace Listing](https://aws.amazon.com/marketplace/pp/prodview-lvfwuztjwe5b2)
  * [FortiTester BYOL Marketplace Listing](https://aws.amazon.com/marketplace/pp/prodview-pvuqutzfg7lke)
3.	The solution requires 3 EIPs to be created so ensure the AWS region being used has available capacity.  Reference [AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html) for more information on EC2 resource limits and how to request increases.
4.	Ensure the BYOL licenses have been registered on the support site.  Reference the VM license registration process PDF in this [KB Article](http://kb.fortinet.com/kb/microsites/search.do?cmd=displayKC&docType=kc&externalId=FD32312).
5.  **If testing a c5/c5n/c6i.4xlarge or bigger instance type, you will need to configure your affinity interrupts.**  Reference the affinity-interrupts.txt for the instance type you are using in the content folder.  Here are the direct links to the current files:
  * [c5.4xlarge](./content/c5.4xlarge-affinity-interrupts.txt)
  * [c5.9xlarge](./content/c5.9xlarge-affinity-interrupts.txt)
  * [c5.18xlarge](./content/c5.18xlarge-affinity-interrupts.txt)
  * [c5n.4xlarge](./content/c5n.4xlarge-affinity-interrupts.txt)
  * [c5n.9xlarge](./content/c5n.9xlarge-affinity-interrupts.txt)
  * [c5n.18xlarge](./content/c5n.18xlarge-affinity-interrupts.txt)
  * [c6i.4xlarge](./content/c6i.4xlarge-affinity-interrupts.txt)
  * [c6i.8xlarge](./content/c6i.8xlarge-affinity-interrupts.txt)
  * [c6i.16xlarge](./content/c6i.16xlarge-affinity-interrupts.txt)
  * [c6i.24xlarge](./content/c6i.24xlarge-affinity-interrupts.txt)

Once the prerequisites have been satisfied, login to your account in the AWS console and proceed with the deployment using the default values where present.

Use the CloudFormation outputs to get the login URLs for the Gateway FortiGate, DUT FortiGate, and FortiTester.  To access the webservers, use SSLVPN (Tunnel recommended but Web Mode available) to connect to the Gateway FortiGate and then you can SSH to the 4x web server VIPs configured on the DUT FortiGate (ie 10.0.1.251-10.0.1.254).