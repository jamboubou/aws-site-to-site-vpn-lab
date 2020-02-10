# Module 1: VPN Configuration

In this module, you will create and configure a Customer Gateway, an AWS Transit Gateway,  and a VPN Site-To-Site connection.

## 1. Create and configure Customer Gateway

Use the AWS console.

> The console's region will default to the last region you were using previously. 

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **VPC** under Networking & Content Delivery.

2. Choose **Customer Gateways** on the left pan under 'Virtual Private Network (VPN)' section.

3. Choose **Create Customer Gateway**.

4. Input `OnPrem VPN Gateway` as the 'Name'.

5. Choose **Dynamic** for Routing.

6. Input the IP Address noted from the last step of section 1 in Module 1 as the 'IP Address'.

7. Leave other fields with default settings and choose **Create Customer Gateway**.

</p></details>

## 2. Create and configure AWS Transit Gateway

Use the AWS console.

> The console's region will default to the last region you were using previously. 

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **VPC** under Networking & Content Delivery.

2. Choose **Transit Gateways** on the left pan under 'Transit Gateways' section.

3. Choose **Create Transit Gateway**.

4. Input `VPN Transit Gateway` as the 'Name Tag' and 'Description'.

5. Leave other fields with default settings and choose **Create Transit Gateway**.

6. Choose **Transit Gateway Attachments** on the left pan under 'Transit Gateways' section.

7. Choose **Create Transit Gateway Attachment**.

8. Select the Transit Gateway we just created from the drop-down menue of **Transit Gateway ID**.

9. Select **VPC**.

10. Input `VPC Attachment` as the 'Attachment name tag'.

11. Select the xxxx-aws-vpc from the drop-down menue of **VPC ID**.

12. Leave other fields with default settings and choose **Create attachment**.

</p></details>

## 3. Create and configure Site-To-Site VPN

Use the AWS console.

> The console's region will default to the last region you were using previously. 

<details>
<summary><strong>Step-by-step instructions (expand for details)</strong></summary><p>

1. In the AWS Management Console choose **Services** then select **VPC** under Networking & Content Delivery.

2. Choose **Site-To-Site VPN Connections** on the left pan under 'Virtual Private Network (VPN)' section.

3. Choose **Create VPN Connection**.

4. Input `OnPrem AWS VPN Connection` as the 'Name Tag'.

5. Choose **Transit Gateway** as 'Target Gateway Type'.

6. Select the Transit Gateway we just created from the drop-down menue of **Transit Gateway**.

7. Choose **Existing** as 'Customer Gateway'.

8. Select the Customer Gateway we just created from the drop-down menue of **Customer Gateway ID**.

9. Leave other fields with default settings and choose **Create VPN Connection**. Choose **Close**.

10. Select the VPN Connection we just created and choose **Download Configuration** on the top.

11. Select **Generic** as 'Vendor' and choose **Download**. Keep the VPN Connection configuration file, we will use it in Module 3.

12. The 'state' will remain pending and if you choose **Tunnel Details** from the bottom you can see both 'Tunnel 1' and 'Tunnel 2' are down which is expected as we didn't yeh create the Customer VPN Gateway.

</p></details>


## Next

You may now proceed to [Module 3 - VPN Gateway Deployment](../3_VPNGatewayDeployment).
