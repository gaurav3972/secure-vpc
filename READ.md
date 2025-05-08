# 🔐 **AWS Dual-Zone Secure VPC:**

**"AWS VPC Secure Architecture with Public and Private Subnets"**

In this project, we will design a VPC with a public subnet, a private subnet, and a Network Address Translation (NAT) device in the public subnet.
***

### 📄 **Project Description:**

"AWS VPC Secure Architecture with Public and Private Subnets"

* **VPC** creation with custom IP range
* **Internet Gateway** for public connectivity
* **Public and Private Subnets** with appropriate routing and security rules
* **Security Groups** to define instance-level traffic controls
* **Network Access Control Lists (NACLs)** to enforce subnet-level security
* **Bastion Host** for secure SSH access to private instances
* **NAT Gateway** to allow outbound internet access from private subnets
* **Route Tables** for proper routing between public, private, and internet-connected resources

### 📋 Prerequisites:
* Basic **EC2** knowledge

* Understanding of **VPCs, subnets, and route tables**

* Familiarity with **IP addressing, Security Groups, and NACLs**
***

### 🔐Step 1: Accessing the AWS Console
To begin setting up your secure VPC, you need to log in to your AWS account:

1. Go to the official AWS login page:
👉 https://aws.amazon.com/console/

2. Click "Sign In to the Console".

3. Choose your login method:

* Use your Root user email (for full account access)

* Or sign in with an IAM user (if you’re part of an organization)

4. Enter your password and click Sign In.
Of course! Here's a **reworded version** of **Step 2** with a fresh approach:

*** 

## 🛠️ **Step 2: Creating Your VPC**

To begin defining your network infrastructure, you need to create a Virtual Private Cloud (VPC). Follow these steps:

1. Open the **AWS Management Console** and log in to your account.

2. In the top search bar, type **“VPC”** and click on the **VPC** option under **Services**.

3. On the left sidebar, select **Your VPCs**.

4. Click the **Create VPC** button to start the VPC creation process.

5. In the **Create VPC** wizard, fill in the following details:

   * **Resources to create:** Choose **VPC only**
   * **Name tag:** Enter **VPC-project**
   * **CIDR block:** Use **10.0.0.0/16**
   * **Tenancy:** Select **Default**

6. Scroll to the bottom and click on **Create VPC** to finalize your VPC setup.
*** 
Here's a completely **reworded version** with a different tone and approach:

---

## 🌐 **Step 3: Set Up and Attach an Internet Gateway**

To enable your VPC to communicate with the internet, follow these steps to create and attach an **Internet Gateway**:

1. In the **VPC Dashboard**, click **Internet Gateways** on the left panel.
2. Click **Create Internet Gateway**.
3. Name the gateway **demo-gw** and hit **Create**.
4. Once created, click **Actions** > **Attach to VPC**.
5. Choose your **VPC-project** and click **Attach Internet Gateway**.

Now, your **public subnet** instances can reach the internet through the gateway.
***
Here’s a **more engaging and streamlined version** of **Step 4** with a fresh approach:

---

## 🌐 **Step 4: Setting Up a Public Subnet**

A **Public Subnet** is essential for hosting resources that need direct access to the internet, like web servers or load balancers. Let's create and configure it for you!

### **Create the Public Subnet:**

1. In the **VPC Dashboard**, click on **Subnets** in the left sidebar.
2. Hit **Create Subnet** to start the process.
3. Fill in the details:

   * **VPC:** Select your **VPC-project**
   * **Subnet Name:** Name it **Public-A**
   * **Availability Zone:** Choose **us-west-2a**
   * **CIDR Block:** Enter **10.0.20.0/24**
4. Click **Create Subnet**. You're halfway there!

### **Configuring the Public Route Table:**

1. In the **VPC Dashboard**, go to **Route Tables** and click **Create Route Table**.

   * **Name:** Enter **PublicRouteTable**
   * **VPC:** Select **VPC-project**

2. Click **Create Route Table** to continue.

3. Now, go to the **Routes** tab and hit **Edit Routes** > **Add Route**.

   * **Destination:** Set it to **0.0.0.0/0** (for internet access).
   * **Target:** Select **Internet Gateway**, then pick **demo-gw**.

4. Click **Save Changes**.

### **Associate the Route Table with the Public Subnet:**

1. Navigate to **Subnets**, select your **Public-A** subnet.
2. Click the **Route Table** tab, then click **Edit Route Table Association**.
3. From the dropdown, choose **PublicRouteTable** and confirm **demo-gw**.
4. Hit **Save** and you're done!

Now, your **public subnet** is connected to the internet via the route table and gateway. Resources in this subnet will have full access to the web!
***
Here's a **refreshed and engaging version** of **Step 5**, with a more dynamic approach:

---

## 🚀 **Step 5: Launching Your Bastion Host**

A **Bastion Host** is your secure gateway to access instances within private subnets, typically by using SSH (or RDP). It acts as a jump server, where you connect first, then reach other instances behind the scenes. Let’s set up your bastion host in the public subnet!

### **Launch the Bastion Host Instance:**

1. **Open the AWS Management Console**, search for **EC2**, and select **EC2** under **Services**.

2. Click on **Instances** in the left-hand menu, then hit **Launch Instance** to create your bastion host.

3. In the **Name and tags** section, enter **bastion** as the name.

4. Under **Application and OS Images**, select **Amazon Linux** from the **Quick Start** tab.

5. In the **Instance Type** section, keep the default **t2.micro** instance type.

6. Under **Key Pair**, select the key pair you'll use to SSH into the instance.

7. Now, configure your **Network Settings**:

   * **VPC:** Choose **VPC-project**
   * **Subnet:** Select **Public-A | us-west-2a**
   * **Auto-assign Public IP:** Select **Enable** (important for SSH access)

8. For **Firewall (Security Group)** settings, click **Create a new security group** and configure:

   * **Security Group Name:** Enter **SG-bastion**
   * **Description:** Enter **SG for Bastion Host - SSH access only**
   * **Inbound Rule:** Set to **SSH** (TCP, Port 22)
   * **Source:** Set to **0.0.0.0/0** (or better, choose **My IP** for enhanced security)

9. Once configured, click **Review and Launch**.

10. **Launch the instance** and wait for it to initialize
***
### **What You’ve Done:**
You’ve now created an **EC2 instance** with a public IP in your public subnet, serving as a **Bastion Host**. This instance will be used to securely access private instances behind the scenes.
***
Here's a **reworded and engaging version** of **Step 6**, designed to be more dynamic and appealing:

---

## 🔒 **Step 6: Creating a Private Subnet**

A **Private Subnet** is perfect for hosting resources that shouldn’t be exposed to the internet, such as backend databases or application servers. Let’s build your private subnet and set up its routing!

### **Create the Private Subnet**

1. **Search for "VPC"** in the AWS Management Console, then click on **VPC** under **Services**.

2. Select **VPC-project** in the **Filter by VPC** dropdown.

3. In the left sidebar, click **Subnets** to view existing subnets.

4. Click **Create Subnet** and enter the following details:

   * **VPC ID:** Select **VPC-project**
   * **Subnet Name:** Enter **Private-A**
   * **Availability Zone:** Choose **us-west-2a**
   * **CIDR Block:** Enter **10.0.10.0/24**

5. Click **Create Subnet**.

By default, this subnet is assigned to the default **VPC Route Table** and **Network ACL**, but it's not publicly accessible since it doesn’t have a direct route to the internet.

---

### **Set Up the Private Route Table**

1. In the left panel, click on **Route Tables**, then click **Create Route Table**.

2. Enter the following:

   * **Name:** Enter **PrivateRouteTable**
   * **VPC:** Select **VPC-project**

3. Click **Create Route Table**.

4. In the **PrivateRouteTable** details page, go to the **Routes** tab and click **Edit Routes**.

5. Add a temporary route:

   * **Destination:** Enter **0.0.0.0/0**
   * **Target:** Select **Internet Gateway**, then choose **demo-gw**.

   *(Note: This will be updated later to point to a NAT device for secure internet access.)*

6. Click **Save Changes**.

---

### **Associate the Route Table with the Private Subnet**

1. Go back to **Subnets**, select your **Private-A** subnet.

2. In the **Route Table** tab, click **Edit Route Table Association**.

3. From the **Route Table ID** dropdown, select **PrivateRouteTable**.

4. Click **Save**.

### **What You've Done:**

You’ve successfully created a **Private Subnet** with its own **Route Table**. Although it currently routes traffic through the Internet Gateway, this will be updated later to use a **NAT device**, ensuring secure internet access for your private resources.
Here’s a **reworked and more engaging version** of **Step 7**, designed to keep things clear but more dynamic and interesting:

---

## 🔒 **Step 7: Creating a Network ACL for Your Private Subnet**

A **Network ACL (NACL)** is an added layer of security for your subnets. It acts like a firewall, controlling inbound and outbound traffic. While the default NACL allows all traffic, creating a custom one for your private subnet enhances control over network access.

### **Create the Network ACL**

1. In the **VPC Dashboard**, go to the left navigation panel and click **Network ACLs** under **Security**.

2. Click on **Create Network ACL**.

3. In the creation dialog, set the following:

   * **Name:** Enter **Private-NACL**
   * **VPC:** Select **VPC-project** from the dropdown.

4. Hit **Create Network ACL**.

---

### **Associate the NACL with the Private Subnet**

1. Once your **Private-NACL** is created, select it from the **Network ACLs** list.

2. Go to the **Subnet Associations** tab and click **Edit Subnet Associations**.

3. Check the box for the **Private-A** subnet to associate it with the new **Private-NACL**.

4. Click **Save Changes**.

---

### **What You've Done:**

You’ve successfully created a **custom Network ACL** and associated it with your **Private Subnet**. This allows you to better control the traffic to and from your private resources for an added layer of security.
***

## 🛡️ **Step 8: Adding Rules to Your Private Network ACL**

Now it’s time to add some **security rules** to your **Private Network ACL**. These rules will control both the inbound and outbound traffic to and from your private subnet, giving you granular control over what’s allowed and blocked.

### **Configure Inbound Rules**

1. In the **VPC Dashboard**, navigate to **Network ACLs** under **Security**.

2. Select **Private-NACL** from the list.

3. Click on the **Inbound Rules** tab and hit **Edit Inbound Rules**.

4. Add your first inbound rule:

   * **Rule Number:** Enter **100**
   * **Type:** Choose **SSH**
   * **Source:** Enter **10.0.20.0/24** (your public subnet’s CIDR block)
   * **Allow / Deny:** Select **Allow**

5. Add the second inbound rule:

   * **Rule Number:** Enter **200**
   * **Type:** Select **Custom TCP Rule**
   * **Port Range:** Enter **1024-65535**
   * **Source:** Enter **0.0.0.0/0**
   * **Allow / Deny:** Select **Allow**

6. Click **Save Changes**.

---

### **Configure Outbound Rules**

1. With **Private-NACL** still selected, switch to the **Outbound Rules** tab and click **Edit Outbound Rules**.

2. Add your first outbound rule:

   * **Rule Number:** Enter **100**
   * **Type:** Select **HTTP**
   * **Destination:** Enter **0.0.0.0/0** (allowing internet traffic)
   * **Allow / Deny:** Select **Allow**

3. Add the second outbound rule:

   * **Rule Number:** Enter **200**
   * **Type:** Select **HTTPS**
   * **Destination:** Enter **0.0.0.0/0**
   * **Allow / Deny:** Select **Allow**

4. Add the third outbound rule:

   * **Rule Number:** Enter **300**
   * **Type:** Select **Custom TCP**
   * **Port Range:** Enter **32768-61000**
   * **Destination:** Enter **10.0.20.0/24** (the CIDR block of your public subnet)
   * **Allow / Deny:** Select **Allow**

5. Click **Save Changes**.

---

### **What You've Done:**

You’ve now fine-tuned the **inbound and outbound rules** for your **Private NACL**. This will help ensure that only authorized traffic can enter or exit your private subnet, while allowing necessary services to communicate with the public subnet and the internet.
***
## 🚀 **Step 9: Launching an EC2 Instance in a Private Subnet**

In this step, we’ll be launching an EC2 instance in your **Private Subnet**, where it will remain isolated from direct internet access, making it more secure. We'll also configure it to be accessed through the **Bastion Host**.

### **Launch the EC2 Instance**

1. In the **AWS Management Console**, search for **EC2** and click the **EC2** result under **Services**.

2. Go to **Instances** in the left-hand menu and click on **Launch Instances**.

3. In the **Name and Tags** section, enter **private** for the instance name.

4. Under **Application and OS Images**, select the **Amazon Linux 2 AMI (HVM) - Kernel 5.10** option from the **Quick Start** tab.

5. In the **Instance Type** section, leave it set to **t2.micro** (default).

6. Under **Key Pair**, select your existing keypair for SSH access.

---

### **Configure Network Settings**

1. In **Network Settings**, click **Edit**, and configure the following:

   * **VPC**: Choose your **VPC-project VPC**.
   * **Subnet**: Select the **Private-A** subnet (the one in your private network).
   * **Auto-assign Public IP**: Disable this (since it's a private subnet).

2. Set up the **Firewall**:

   * Select **Create Security Group**.
   * **Security Group Name**: Enter **SG-Private**.
   * **Description**: “Security group for private subnet instances. Accept SSH inbound requests from Bastion host only.”

   Add two rules:

   * **SSH Rule**:

     * **Type**: SSH
     * **Protocol**: TCP
     * **Port**: 22
     * **Source**: Select **Custom**, then **SG-bastion** (your Bastion Host security group).

   * **HTTPS Rule**:

     * **Type**: HTTPS
     * **Protocol**: TCP
     * **Port**: 443
     * **Source**: Enter **10.0.20.0/24** (your public subnet’s CIDR block).

3. Review and **Launch Instance**.

---

### **Update Security Group Rules for SSH Access**

1. After launching the instance, go to **Security Groups** in the **VPC Dashboard** and locate your **SG-bastion** security group.

2. Switch to the **Outbound Rules** tab and click **Edit Outbound Rules**.

3. Add a rule:

   * **Type**: SSH
   * **Protocol**: TCP
   * **Port**: 22
   * **Destination**: Select **Custom**, then enter the **Security Group ID** of your **SG-Private** security group.

4. **Save Rules**.

---

### **SSH into the Bastion Host & Jump to Private Instance**

Now, let’s SSH into your **Bastion Host** and connect to the **Private EC2 Instance** securely.

1. **Linux/Mac instructions**:

   * Download the **PEM SSH key** file if you haven’t already.

   * In your terminal, ensure the key file’s permissions are correct:

     ```bash
     chmod 400 PEMfilename.pem
     ```

   * Add the private key to the **SSH agent** (this keeps it secure by not copying it to the Bastion Host):

     ```bash
     ssh-add -k PEMfilename.pem
     ```

   * Verify the key is added:

     ```bash
     ssh-add -L
     ```

2. **SSH into the Bastion Host** using your key and forwarding agent:

   ```bash
   ssh -A ec2-user@BastionHostPublicIP
   ```

3. Once inside the Bastion, you can now **SSH** into your **Private EC2 instance** using its private IP (since the Bastion has SSH forwarding enabled).

---

### **What You’ve Achieved:**

* You’ve successfully **launched a private EC2 instance** that mimics a **back-end service**, such as a **database server**, in your private subnet.
* By using the **Bastion Host** as a secure gateway, you’ve **SSH’d into the private instance** without ever directly exposing it to the public internet.
* You’ve set up **SSH agent forwarding** to prevent risky key copies and secure your authentication chain.
***
## 🌐 **Step 10: Setting Up a NAT Gateway for Internet Access in Your Private Subnet**

In this step, we’ll be setting up a **NAT Gateway** that allows EC2 instances in your **Private Subnet** to access the **public internet** (for updates, downloads, etc.) while remaining shielded from direct inbound internet traffic.

Let’s dive in!

### **Step 1: Create a NAT Gateway**

1. **Search for “VPC”** in the AWS Management Console search bar and select **VPC** under **Services**.

2. In the **VPC Dashboard**, click on **NAT Gateways** in the left navigation pane.

3. Hit **Create NAT Gateway** to start the process.

4. **Configure the NAT Gateway settings**:

   * **Name**: Enter **NAT-GW** for clarity.
   * **Subnet**: Select **Public-A** (this ensures the NAT Gateway will sit in your public subnet, allowing access to the internet).
   * **Connectivity type**: Choose **Public** (this allows your NAT Gateway to access the internet).

5. **Allocate an Elastic IP (EIP)** for the NAT Gateway:

   * Click on **Allocate Elastic IP** next to the **Elastic IP Allocation ID**. This is essential for routing traffic from your private instances to the internet.

6. **Create the NAT Gateway**:

   * Hit **Create NAT Gateway** and **wait** for its status to show as **Available**. This may take a few moments.

---

### **Step 2: Update the Route Table for the Private Subnet**

Once the NAT Gateway is available, we’ll update the **Private Route Table** to use the NAT Gateway for outbound traffic.

1. In the **VPC Dashboard**, go to **Route Tables** in the left navigation pane.

2. **Select** your **PrivateRouteTable**.

3. Scroll down and click on the **Routes** tab to view the existing routes.

4. You should have a route with **Destination: 0.0.0.0/0** and **Target** pointing to the **Internet Gateway** (from a previous step). **We need to change this.**

5. **Edit routes**:

   * Find the **0.0.0.0/0** route, and **clear** the existing target by clicking the **X**.
   * Begin typing **NAT**, then select **NAT Gateway** and choose **NAT-GW** from the drop-down menu.

6. Click **Save changes** to complete the update.

---

### **What You’ve Achieved:**

* You've successfully **set up a NAT Gateway**, providing your **private EC2 instances** with secure, outbound internet access (e.g., for updates, downloads).
* You’ve **updated the route table** for your **private subnet**, so now traffic destined for the internet will route through the NAT Gateway instead of the Internet Gateway.
* Your private instances are now **securely isolated** from direct inbound internet access but still able to **reach the internet** for necessary tasks.
***
