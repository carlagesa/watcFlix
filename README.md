# From Code to Cloud: Easily Host Your HTML Website on an EC2 Instance!


Hello my name is Calton, a cloud engineer based in Nairoi, Kenya. In this blog post I will show you how to deploy your html code onto an Amazon EC2. Let's get started.

## Choose An AWS Region.

Before you create an AWS EC2 you need to first choose your AWS region from the top right menu. Remember, you should pick a region that is near you or your customers. This way, your users will have less delay when they access your web applications.

## Setting Up the EC2 Instance:
Search for "EC2" at the top search bar, select "instance" at the left sidebar menu. Then click the “Launch Instance” button to start setting up your server. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2inn33720jumy54b1wkf.png)
Give your instance a name e.g. watchflix-ec2, this name will be displayed in AWS dashboard.
Scroll down to "Application and OS Images (Amazon Machine Image)", under quick start choose "Amazon Linux". The AMI that we are going to use is Amazon Linux 2 AMI Free Tier. For the instance type it will be t2.micro free tier.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ve3pztx01gvh2eo5s9u.png)

Further scroll down to "Key pair (login)", where we are required to select our key pair if it exists otherwise select "create new key pair". Enter the key pair name of your choice, leave the default RSA and .pem as is.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jj4daa65a11fq4jcsewy.png)

Scroll down to Network settings, click Edit. For now most of the inputs except for security groups related inputs will be left as default.
Provide the security group name and description of your choice e.g. watchflix-sg. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ndffwr7pqqrh6e38oj2.png)

Now, pay close attention here. Under "Inbound Security Group Rules" we are going to add two rules.
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3jc7vohaxhj548jtw1lc.png)

The first rule is for port 22 which is already present. Port 22 allows you to SSH into your EC2 instance. Since port 22 allows access into our EC2 we need to change our source type to limit it only to our IP address - this is a security best practice.

The second rule we are going to add is on port 80. Opening this port will allow users on the internet to access our website. Click "Add security group rule" and under type select HTTP from the dropdown menu. Notice how the port changes to 80? OK. For the source type, we are going to allow traffic on port 80 to come from anywhere on the internet. Under Source, select Anywhere on IPv4 — this will bring up 0.0.0/0 automatically 

Once done tweaking, click "Launch instance" at the bottom right part of the page. 

## EC2 Instance Connect
EC2 Instance Connect allows you to connect to Linux instances using SSH without managing SSH public keys. This can improve security, simplify workflow, and improve auditing and compliance.
 
To use the EC2 Connect terminal, you can connect to an Amazon EC2 instance using the Amazon EC2 console:
1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
2. In the navigation pane, select Instances
3. Select the instance and choose Connect
4. Select the EC2 Instance Connect tab
5. For Connection type, choose Connect using EC2 Instance Connect
6. Verify the username
7. Choose Connect to open a terminal window


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vs8thylmsd7aoyp3axtx.png)
Installing a Web Server:
First things first, we need to change to the root user, type `sudo su` on your terminal. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l0r8scoeo90tyz9qfnqa.png)

To update to the latest system security patches type `yum update -y`.
Type `yum install -y httpd` is used to install the Apache HTTP Server on a Linux system, this allows us to serve web content over HHTTP

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2lthqod7bcnriq8imnjj.png)
Next we need to change directory from ec2 to html, to do this type `cd /var/www/html`. Notice the directory change? Good!

We are now going to download file to our EC2 using the Linux command wget. Type `wget https://github.com/carlagesa/watchflix/archive/refs/heads/main.zip`. Confirm that it successfully downloaded the main.zip file, you can do this by typing `ls`

Upon confirmation, we need to unzip (type `unzip main.zip`) and copy all of our unzipped web files into the html directory (type `cp -r watchflix-main/* /var/www/html/`). 
To confirm this you can type `ls` on the terminal.
Also type `rm -rf watchflix-main main.zip` to remove the zipped files we don't require.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/db6iwk9oyz5ryuq3j4vb.png)

And that's basically it, now we just have to start our service using `systemctl enable httpd` and `systemctl start httpd`

Accessing the Web App:
Lets open our EC2 dashbaord and select our instance and go to the details below to copy the Public IPv4 address.Open a new tab and paste the address, Voila! Now we can access our website.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ayrwwwfi0pewwosnhp5n.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8x07p6tmklox8s7icuqi.png)

