# Module 3: VPN Gateway Deployment

In this module, you will deploy the strongSwan VPN Gateway using a CloudFormation template. As a first step,download the [template](../templates/vpn-gtw-template.yaml) 

## 1. Create the VPN Gateway

Use the AWS console to launch the CloudFormation previsouly downloaded.

> The console's region will default to the last region you were using previously. This should match the region where you launched your On Prem environment previously.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **CloudFormation** under Management & Governance.

2. Choose your desired **Region** in top-right of the console if not already selected. This should match the region where you launched your On Prem environment previously.

3. Choose **Stacks**.

4. Choose **Create Stack** in the top right of the console and select **With new resources (standard)**.

5. In the 'Specify template' section, choose **Upload a template file** and click on **Choose file** and upload the [template](../templates/vpn-gtw-template.yaml).

6. Choose **Next**.

7. Open the VPN connection configuration file 'vpn-xxxxxxxx.txt' downloaded from the step 11 of section 3 in Module 2.

8. Input `vpngtw-deployment` as the 'Stack name'.

9. Input `onprem` as the 'Environment' parameter in the 'Parameters section'.

10. Input the name of your organization as the 'Organization Identifier' parameter in the 'Parameters section' or leave default value.

10. Input a name for the VPN application as the 'Application Identifier' parameter in the 'Parameters section' or leave default value.

11. Map the VPN Tunnels parameters with values from the VPN connection configuration file 'vpn-xxxxxxxx.txt'.

|CloudFormation Parameter|Required|Description|VPN Config File Parameter|
|---------|--------|-----------|-------|
|**VPN Tunnel 1**| | | |
|`VPN Tunnel 1 Pre-Shared Key`|Required|See the remote site's configuration for the "IPSec Tunnel #1" section and "Pre-Shared Key" value.|`Pre-Shared Key`|
|`VPN Tunnel 1 Remote External IP Address`|Required|See the remote site's configuration for the "IPSec Tunnel #1" secton, "Outside IP Addresses" section and "Virtual Private Gateway" value.|`Outside IP Addresses:Virtual Private Gateway`|
|`VPN Tunnel 1 Remote Inside CIDR`|Required|See the remote site's configuration for the "IPSec Tunnel #1" secton, "Inside IP Addresses" section and "Virtual Private Gateway" value.|`Inside IP Addresses:Virtual Private Gateway`|
|`VPN Tunnel 1 Local Inside CIDR`|Required|See the remote site's configuration for the "IPSec Tunnel #1" secton, "Inside IP Addresses" section and "Customer Gateway" value.|`Inside IP Addresses:Customer Gateway`|
|`VPN Tunnel 1 BGP ASN`|Optional|See the remote site's configuration for the "BGP Configuration Options" and the "Virtual Private  Gateway ASN" value.|`BGP Configuration Options:Virtual Private  Gateway ASN`|
|`VPN Tunnel 1 BGP Neighbor IP Address`|Required|See the remote site's configuration for the "BGP Configuration Options" and the "Neighbor IP Address" value.|`BGP Configuration Options:Neighbor IP Address`|
|**VPN Tunnel 2**| | | |
|`VPN Tunnel 2 Pre-Shared Key`|Required|See Tunnel 1.|`Pre-Shared Key`|
|`VPN Tunnel 2 Remote External IP Address`|Required|See Tunnel 1.|`Outside IP Addresses:Virtual Private Gateway`|
|`VPN Tunnel 2 Remote Inside CIDR`|Required|See Tunnel 1.|`Inside IP Addresses:Virtual Private Gateway`|
|`VPN Tunnel 2 Local Inside CIDR`|Required|See Tunnel 1.|`Inside IP Addresses:Customer Gateway`|
|`VPN Tunnel 2 BGP ASN`|Optional|See Tunnel 1.|`BGP Configuration Options:Virtual Private  Gateway ASN`|
|`VPN Tunnel 2 BGP Neighbor IP Address`|Required|See Tunnel 1.|`BGP Configuration Options:Neighbor IP Address`|


12. Rest of parameters are summrized below.

|CloudFormation Parameter|Required|Description|Value|
|---------|--------|-----------|-------|
|**Local Network**| | | |
|`VPC ID`|Required|The VPC in which the VPN gateway is to be deployed.|`Select the xxxx-onprem-VPC`|
|`VPC CIDR Block`|Required|The CIDR block of the local VPC. Used to advertise via BGP routing information to the remote site.|`Enter the onprem VPC CIDR`|
|`Subnet ID for VPN Gateway`|Required|The subnet in which the VPN gateway is to be deployed.|`Choose the xxxx-onprem-Private-Subnet`|
|`pUseElasticIp`|Optional|Use elastic IP address?|`false`|
|`pLocalBgpAsn`|Optional|The BGP Autonomous System Number (ASN) used to represent the local end of the site-to-site VPN connection.|`65000`|

13. Leave other fields to default and choose **Next**.

14. Leave the default setting and choose **Next**.

15. Leave the default setting and check the case **I acknowledge that AWS CloudFormation might create IAM resources with custom names.**.

16. Choose **Create stack**.

17. In the AWS Management Console choose **Services** then select **System Manager** under Management & Governance.

18. Choose your desired **Region** in top-right of the console if not already selected. This should match the region where you launched your On Prem environment previously.

19. Choose **Session Manager** under 'Instance & Nodes' on the left pan.

20. Choose **Start Session**.

21. Select **vpngw-onprem** and choose **Start Session**. This will open a new tab in your browser with shell prompt to the selected instance.

22. Check that the tunnels are **ESTABLISHED** by running below command:

```
sudo swanctl --list-sas

``` 

</p></details>

## 2. Configure route tables

Note that even the tunnels are established and healthy in the VPN Gateway, it takes couple of minutes to propagate into the AWS Site-To-Site Connection and show status as 'available'

In the meanwhile we will finalize the route tables configuration.

> The console's region will default to the last region you were using previously. This should match the region where you launched your On Prem environment previously.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **VPC** under Networking & Content Delivery.

2. Choose **Route Tables** on the left pan under 'Virtual Private Cloud' section.

3. Select **xxxx-aws-Private-RT**, choose **Routes** tab from the bottom pan and click on **Edit Routes**. 

4. Choose **Add Route** and input the CIDR of the on prem VPC '10.10.0.0/16' as 'Destination'. Select the Transit Gateway we've created 'VPN Transit Gateway' as 'Target'. 

5. Choose **Save Routes** and then **close**.

6. Select **xxxx-onprem-Private-RT**, choose **Routes** tab from the bottom pan and click on **Edit Routes**. 

7. Choose **Add Route** and input the CIDR of the aws VPC '10.20.0.0/16' as 'Destination'. Select the VPN Gateway instance we've created 'vpn-gw-onprem' as 'Target'. 

8. Choose **Save Routes** and then **close**.

</p></details>

## 3. Tests

Ok let's now redo our previous tests and try to reach our AWS EC2 instance from our emulated On Prem EC2 instance. 

You can check that the Site-To-Site VPN Connection, we configured show status "UP" for both tunnels.

> The console's region will default to the last region you were using previously. This should match the region where you launched your On Prem environment previously.

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **System Manager** under Management & Governance.

2. Choose your desired **Region** in top-right of the console if not already selected. This should match the region where you launched your On Prem environment previously.

3. Choose **Session Manager** under 'Instance & Nodes' on the left pan.

4. Choose **Start Session**.

5. Select **test-onprem** and choose **Start Session**. This will open a new tab in your browser with shell prompt to the selected instance.

6. Go to the previous browser tab and in the AWS Management Console choose **Services** then select **EC2** under Compute.

7. Choose **Running Instances** and select **test-aws**. Copy the 'Private IPs' value from the 'Description' tab in the bottom pan. 

	![AWS EC2 Private IP](../images/aws-ec2-privateip.png)

8. Back to the shell session browser tab to our On Prem instance from step 5, run the command:
```
ping <test-aws instance private IP>
``` 
9. The ping command now should succeed. 

</p></details>

## TODO

1. Add Network Manager Monitoring Module
2. Add screenshots
3. Add multi AZs Module for HA

