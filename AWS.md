Step 1 - Create security groups

- Elastic Load Balancer

- TomCat Instance

- Backend Services (memcache, mysql, rabbit mq)


Step 2 - Create Key Pairs


Step 3 - Clone the source code and git check out into awsliftandshift directory.

Command : git clone https://github.com/devopshydclub/vprofile-project.git

Step 4 - Review bash scripts using Sublime text editor

Step 5 - Launch the backend EC2 instances first, copy the bash script from sublime text and paste in userdata box.

Step 6 - Connect to each of the backend instances to via ssh, switch to root user and check their status.

Step 7 - Setup Route 53


Step 8 -Create record for the Route 53 and add Private IP address for the three backend services.


Step 9 - Launch the EC2 instance for tomcat application server, copy its script from sublime text to paste in userdata box.

Step 10 - Open powershell to install JDk8 and Maven for build & deploy artifacts.

Command: choco install jdk8 -y
Command: choco install maven -y

Step 11 - Update application.properties by going into resources directory and change db name as specified in Route 53

Step 12 - - Go to vprofile directory and install the artifacts in it using mvn install command to generate artifacts

Command: mvn install

Step 13 -Open powershell and install AWSCLI. This will enable us store the artifact in s3 bucket.

Command: choco install awscli -y

Step 14 - Create an access key in AWS IAM for the s3 bucket.

Step 15 - Configure AWS in git bash using command aws configure.

Step 16 - Create an s3 bucket in the command line.

Step 19 - Copy the artifact in target directory and transfer into the s3 bucket created.

Step 20 - Create a IAM role in order for tomcat EC2 instance to download the artifact from s3 bucket.

Step 21 - log on to the tomcat server using the ssh key.
confirm status of tomcat

switch to root

cd /var/lib/tomcat8/webapps

stop the tomcat service

delete the root file 

Step 22 - Download the Artifact to the Tomcat Instance

install aws cli 
Command: apt install awscli

copy the file from aws s3 to the server
Command: aws s3 cp s3://profile-artifact-storage/vprofile-v2.war /tmp/vprofile-v2.war

Copy to /var/lib/tomcat8 
Command: cp vprofile-v2.war /var/lib/tomcat8/webapps/ROOT.war

start the tomcat8
systemctl start tomcat8

Step 23 - Create Target group and application load balancer
- Target Group

- Application Load Balancer

- Copy the Endpoint of the load balancer and create cname record @ godaddy dns 


Step 24- Create Auto Scaling Group for the EC2 instance.

- Create image from the tomcat instance
- Create launch configuration
- Create Auto Scaling Group

Step 25 - Launch Website
