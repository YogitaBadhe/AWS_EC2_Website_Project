# Host a Website on EC2 Linux Instance Using Apache

This guide walks you through the process of hosting a static website on an **Amazon EC2 instance** running **Amazon Linux 2**. The website will be served using the **Apache HTTP server**.

## **Description**

In this project, you'll launch an **EC2 instance** with **Amazon Linux 2**, install **Apache HTTP server**, deploy a simple static website (HTML files), and manage the website using **Git**.

## **Prerequisites**

Before you begin, ensure that you have the following:

1. **AWS Account**: You need an active AWS account to create EC2 instances and use other AWS services.
2. **IAM User Permissions**: Your IAM user should have sufficient permissions to create EC2 instances, configure security groups, and manage related resources.
3. **SSH Key Pair**: You need an SSH key pair for secure access to your EC2 instance.
4. **A Web Browser**: You'll need a browser to test your website by accessing the EC2 public IP.
5. **Basic Linux and SSH Knowledge**: Comfort with terminal commands to interact with your EC2 instance.
6. **Git Installed**: Ensure **Git** is installed on your local machine and EC2 instance.

## **Steps**

### 1. **Launch an EC2 Instance**

1. **Login to AWS Console**: Navigate to the [AWS Management Console](https://aws.amazon.com/console/) and log in.
2. **Navigate to EC2**: In the console, search for and select **EC2**.
3. **Launch Instance**: 
    - Click **Launch Instance**.
    - **Choose an AMI**: Select **Amazon Linux 2 AMI** (Amazon Linux 2 is the default for EC2).
    - **Choose Instance Type**: Select a free tier-eligible instance type like **t2.micro**.
    - **Configure Instance**: Use the default configuration or customize based on your needs.
    - **Configure Security Group**:
        - Add a rule to allow **HTTP (Port 80)** for web traffic.
        - Add a rule for **SSH (Port 22)** for secure access (limit to your IP for security).
    - **Review and Launch**: Review your settings and click **Launch**.

### 2. **Connect to Your EC2 Instance via SSH**

After your EC2 instance is running, you can connect to it using SSH:

1. **Find the Public IP**: Go to the **Instances** page in EC2 and locate your instance. Copy the **Public IP** address.
2. **SSH into the Instance**:
   - Open your terminal.
   - Run the following command to connect to your EC2 instance (replace `<your-key.pem>` with your private key and `<ec2-public-ip>` with your EC2 public IP):
     ```bash
     ssh -i /path/to/your-key.pem ec2-user@<ec2-public-ip>
     ```
   - Accept the SSH fingerprint if prompted to establish a secure connection.

### 3. **Install Apache Web Server and Git**

Once you're connected to your EC2 instance, you'll need to install the **Apache HTTP server** and **Git**.

1. **Update the System**:
   ```bash
   sudo yum update -y
   ```

2. **Install Apache**:
   ```bash
   sudo yum install httpd -y
   ```

3. **Install Git**:
   ```bash
   sudo yum install git -y
   ```

### 4. **Start Apache Web Server**

After installation, start the Apache service and enable it to automatically start on boot.

1. **Start Apache**:
   ```bash
   sudo systemctl start httpd
   ```

2. **Enable Apache to Start on Boot**:
   ```bash
   sudo systemctl enable httpd
   ```

### 5. **Clone Website Repository from GitHub**

To deploy the website, you need to clone the repository containing your website files.

1. **Navigate to the Apache root directory**:
   ```bash
   cd /var/www/html/
   ```

2. **Clone the GitHub Repository**:
   ```bash
   sudo git clone <your-git-repository-url> .
   git clone https://github.com/YogitaBadhe/AWS_EC2_Website_Project.git
   cd AWS_EC2_Website_Project
   ```

3. **Set the Correct Permissions**:
   ```bash
   sudo chown -R apache:apache /var/www/html/
   ```

### 6. **Configure Git to Push Changes**

1. **Set Up Git Configuration**:
   ```bash
   git config --global user.name "YogitaBadhe"
   git config --global user.email "ybadhe0990@gmail.com"
   ```

2. **Make Changes to the Website**:
   ```bash
   index.html
   ```

3. **Commit and Push Changes**:
   ```bash
   git add .
   git commit -m "Initial website commit"
   git push origin main
   ```

### 7. **Configure the Security Group**

To allow access to your website, make sure your EC2 instance’s security group is configured correctly.

1. **Edit Security Group**:
   - Go to **EC2 Console > Security Groups**.
   - Select the security group associated with your EC2 instance.
   - Ensure there’s an inbound rule allowing **HTTP (Port 80)** from **0.0.0.0/0** (for public access) or your IP range.

### 8. **Access the Website**

Now that the files are deployed and the server is configured, open a browser and go to the **Public IP** of your EC2 instance:
```
http://<ec2-public-ip>
```
You should see your website in the browser!

### 9. **Optional: Set Up a Domain Name**

If you have a domain name and want to map it to your EC2 instance:

1. **Allocate an Elastic IP**: 
   - In the EC2 console, allocate an **Elastic IP** and associate it with your EC2 instance.
   
2. **Update DNS Settings**: 
   - Log into your domain registrar’s control panel.
   - Set the **A Record** for your domain to point to your Elastic IP.
   - Propagation may take some time (typically up to 24 hours).

### 10. **Stop/Restart Apache Web Server (Optional)**

1. **To stop Apache**:
   ```bash
   sudo systemctl stop httpd
   ```

2. **To restart Apache** (for changes like configuration updates):
   ```bash
   sudo systemctl restart httpd
   ```

---

## **Additional Resources**

- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/index.html)
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [Amazon Linux 2 AMI Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-versions.html)
- [GitHub Documentation](https://docs.github.com/en/get-started)

---

This **README.md** provides step-by-step instructions to host a static website on an EC2 instance running **Amazon Linux 2**, using **Apache HTTP server**, and managing the website with **Git**.

