# MATLAB on Amazon Web Services (Windows VM)

## Step 1. Launch the Template

Click the **Launch Stack** button to deploy a standalone MATLAB&reg; desktop client on AWS&reg;. This 
opens the CloudFormation Create Stack screen in your web browser.

| Region | Launch Link |
| --------------- | ----------- |
{% for region in regions -%}
| **{{ region }}** | [![alt text](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png "Start a MATLAB instance using the template")](https://{{ region }}.console.aws.amazon.com/cloudformation/home?region={{ region }}#/stacks/create/review?templateURL={{ template_url }}) |
{% endfor %}

**Note**: Creating a stack on AWS can take a few minutes.

## Step 2. Configure the Cloud Resources
After you click the Launch Stack button above, the “Create stack” page will open in your browser where you can configure the parameters. It is easier to complete the steps if you position these instructions and the AWS console window side by side.

1. Specify a stack name. This will be shown in the AWS CloudFormation console and must be unique within the AWS account.


2. Specify and check the defaults for these resource parameters:

| Parameter label | Description |
| --------------- | ----------- |
{% for parameter in parameters -%}
| **{{ parameter.label }}** | {{ parameter.description }} |
{% endfor %}

>**Note**: In the capabilities section, you must acknowledge that AWS Cloudformation might create IAM resources and autoexpand nested templates when creating the stack.

3. Click the **Create Stack** button.  The CloudFormation service will start creating the resources for the stack. <p>After clicking **Create** you will be taken to the *Stack Detail* page for your stack. Wait for the Status to reach **CREATE\_COMPLETE**. This may take up to 10 minutes.</p>

## Step 3. Connect to the Virtual Machine in the Cloud

To connect to the Virtual Machine (VM) using Remote Desktop Client, follow these steps:
1. Expand the **Outputs** section in the *Stack Detail* page.
2. Look for the key named `RDPSSHConnection` and copy the corresponding public DNS name listed under value. *For example*: ec2-11-222-33-44.compute-1.amazonaws.com, or ip-11-222-33-44.ec2.internal, for a private instance.
3. Launch any remote desktop client, paste the public DNS name in the appropriate field, and connect. On the Windows Remote Desktop Client you need to paste the public DNS name in the **Computer** field and click **Connect**.
4. In the login screen that's displayed, use the username `Administrator` and the password you specified while setting up the stack in [Step 2](#step-2-configure-the-stack).
5. You can also connect using SSH from the terminal using the format: `ssh Administrator@<DNS name>`. *For example*: ssh Administrator@ec2-11-222-33-44.compute-1.amazonaws.com

If you choose to enable browser access for MATLAB, then:
1. Expand the **Outputs** section in the *Stack Details* page.
2. Look for the key named `BrowserConnection` and click on it
3. On the login screen, use the password you specified while deploying the stack in [Step 2](#step-2-configure-the-stack) as the 'auth token' to authenticate.
4. Browser access for MATLAB is enabled using `matlab-proxy`, a MathWorks&reg; developed Python&reg; package. For more information on `matlab-proxy`, refer to [matlab-proxy GitHub repository](https://github.com/mathworks/matlab-proxy).

## Step 4. Start MATLAB
Double-click the MATLAB icon on the virtual machine desktop to start MATLAB. The first time you start MATLAB, you need to enter your MathWorks Account credentials to license MATLAB. For other ways to license MATLAB, see [MATLAB Licensing in the Cloud](https://www.mathworks.com/help/install/license/licensing-for-mathworks-products-running-on-the-cloud.html). 

>**Note**: It may take up to a minute for MATLAB to start the first time.


# Additional Information

## Configure Private Network
To set up a private networking configuration for the MATLAB EC2 instance, you can set the `EnablePublicIPAddress` parameter to `No` to avoid attaching a public IP to the MATLAB EC2 instance. Ensure these requirements before deploying your stack:

- **Client Access**: Specify the private IP addresses of the jumpbox or client(s) that will access the MATLAB EC2 instance, using the `ClientIPAddress` parameter. 
- **Online Licensing**: To use online licensing for MATLAB, your MATLAB EC2 instance must be able to access domains at `*.mathworks.com`. Allow outbound access to these domains by setting up an appropriate method in your VPC.
- **Stack Deployment**: Without a public IP, the MATLAB EC2 instance might get stuck in the `CREATE_IN_PROGRESS` state during stack deployment. To avoid this, ensure that your VPC has a NAT Gateway or a [VPC endpoint for the AWS CloudFormation service (AWS)](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/vpc-interface-endpoints.html#vpc-endpoint-create) before deployment.
- **CloudWatch Logs**: To deliver CloudWatch logs from the instance, ensure that your VPC has either a NAT Gateway or a [VPC endpoint for CloudWatch service (AWS)](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html#create-VPC-endpoint-for-CloudWatchLogs).

To easily create a VPC with these settings, you can use the [VPC CloudFormation Template](https://github.com/mathworks-ref-arch/iac-building-blocks/tree/main/aws/vpc-template/v1/README.md).

## CloudWatch Logs
CloudWatch logs enables you to access logs from all the resources in your stack in a single place. To use CloudWatch logs, launch the stack with the feature "Configure cloudwatch logging for the MATLAB instance" enabled. Once the stack deployment is complete, you can access your logs in the "Outputs" of the stack by clicking the link next to "CloudWatchLogs". Note that if you delete the stack, the CloudWatch log group is also deleted. For more information, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html).

## Installing NVIDIA GRID Drivers
If you are running a GPU instance that belongs to G3, G4dn or G5 instance type, you can install NVIDIA GRID Drivers using the `Install-NVIDIAGridDriver.ps1` script placed in the `C:\Windows\NVIDIADrivers\` directory. These drivers are certified to provide optimal performance for visualization applications that render content such as 3D models or high-resolution videos. Possible use cases include 4K resolution streaming via NICE DCV or using Automated Driving toolbox to run simulations in a rich 3D-environment. Please note that, for the installation of GRID drivers to complete, a machine restart is required.

## Delete Your Cloud Resources

Once you have finished using your stack, it is recommended that you delete all resources to avoid incurring further cost. To delete the stack, do the following:
1. Log in to the AWS Console.
1. Go to the AWS CloudFormation page and select the stack you created.
1. Click the **Actions** button and click **Delete Stack** from the menu that appears.

## Nested Stacks

This CloudFormation template uses nested stacks to reference templates used by multiple reference architectures. For details, see the [MathWorks Infrastructure as Code Building Blocks](https://github.com/mathworks-ref-arch/iac-building-blocks) repository.

----

Copyright 2020-2025 The MathWorks, Inc.

----
