# AWS-Lift-Shift-Project
In this project, I set up an ELB, TomCat Instance alongside backend services(memcache, mysql, rabbit mq). I used Maven to build the artifact, created a S3 Bucket to store the artifact. Deployed the artifact to the TomCat Instance and Load balanced the website with AWS application load balancer and created an Auto Scaling Group.

# STEPS

Step 1 - Create security groups

- Elastic Load Balancer
![1](https://user-images.githubusercontent.com/52894481/184719851-8c1b5d0e-2560-437b-a563-8125334a7ee2.JPG)

- TomCat Instance
![2](https://user-images.githubusercontent.com/52894481/184719683-88089b2d-fe9c-4810-8fed-d09106ed729b.JPG)

- Backend Services (memcache, mysql, rabbit mq)
![3](https://user-images.githubusercontent.com/52894481/184719650-cc5a4511-c123-41b0-b159-8c94bd87856c.JPG)

Step 2 - Create Key Pairs
![4](https://user-images.githubusercontent.com/52894481/184720077-a26c41cd-a39d-44fe-9ba8-acb881a1c197.JPG)

Step 3 - Clone the source code and git check out into awsliftandshift directory.
Command : git clone https://github.com/devopshydclub/vprofile-project.git
![5](https://user-images.githubusercontent.com/52894481/184720138-95964399-0854-4f9e-8387-1850f581bf4d.JPG)

Step 4 - Review bash scripts using Sublime text editor
![6](https://user-images.githubusercontent.com/52894481/184720171-d70cae0b-3d14-4cbd-af11-886c9a29d3d9.JPG)

Step 5 - Launch the backend EC2 instances first, copy the bash script from sublime text and paste in userdata box.
![9](https://user-images.githubusercontent.com/52894481/184720292-45e6b4c0-fc66-4032-9c47-c62f8490d539.JPG)

Step 6 - Connect to each of the backend instances to via ssh, switch to root user and check their status.

Step 7 - Setup Route 53
![10](https://user-images.githubusercontent.com/52894481/184720341-9103599a-985f-4d61-89d1-02796551e7f4.JPG)

Step 8 -Create record for the Route 53 and add Private IP address for the three backend services.
![11](https://user-images.githubusercontent.com/52894481/184720405-315cc535-a4c1-4679-83f0-89f830acf00a.JPG)

Step 9 - Launch the EC2 instance for tomcat application server, copy its script from sublime text to paste in userdata box.

Step 10 - Open powershell to install JDk8 and Maven for build & deploy artifacts.

Command: choco install jdk8 -y
Command: choco install maven -y

Step 11 - Update application.properties by going into resources directory and change db name as specified in Route 53
![12](https://user-images.githubusercontent.com/52894481/184720596-6d29cb9e-0c52-49f5-9a64-f317ba3fee1e.JPG)

Step 12 - - Go to vprofile directory and install the artifacts in it using mvn install command to generate artifacts

Command: mvn install
![13](https://user-images.githubusercontent.com/52894481/184720658-ba06ce42-8e8c-4f5b-bb2a-31dd04d98a43.JPG)

Step 13 -Open powershell and install AWSCLI. This will enable us store the artifact in s3 bucket.

Command: choco install awscli -y

Step 14 - Create an access key in AWS IAM for the s3 bucket.
![14](https://user-images.githubusercontent.com/52894481/184720726-08678d27-b3b5-43c2-8fe7-bb2891e6e00b.JPG)

Step 15 - Configure AWS in git bash using command aws configure.
![15](https://user-images.githubusercontent.com/52894481/184720771-b465e241-9fc2-4175-89eb-4ad241dc605c.JPG)

Step 16 - Create an s3 bucket in the command line.
![16](https://user-images.githubusercontent.com/52894481/184720812-7a1cfc58-5e5d-4c24-9e17-f5a0b8d6d090.JPG)

Step 17 - Copy the artifact in target directory and transfer into the s3 bucket created.
![17](https://user-images.githubusercontent.com/52894481/184720861-4948b181-53ba-42bd-a3d6-f980198e34e4.JPG)

Step 18 - Create a IAM role in order for tomcat EC2 instance to download the artifact from s3 bucket.
![18](https://user-images.githubusercontent.com/52894481/184721193-a91b8813-a86b-45b7-9d52-5dfe8c54e859.JPG)

Step 19 - log on to the tomcat server using the ssh key.
confirm status of tomcat

switch to root

cd /var/lib/tomcat8/webapps

stop the tomcat service

delete the root file 
![19](https://user-images.githubusercontent.com/52894481/184721233-d82fae36-7a76-43dc-bb7e-f0522bb95cb5.JPG)

Step 20 - Download the Artifact to the Tomcat Instance

install aws cli 
Command: apt install awscli

copy the file from aws s3 to the
server
Command: aws s3 cp s3://profile-artifact-storage/vprofile-v2.war /tmp/vprofile-v2.war

Copy to /var/lib/tomcat8 
Command: cp vprofile-v2.war /var/lib/tomcat8/webapps/ROOT.war

start the tomcat8
systemctl start tomcat8

Step 21 - Create Target group and application load balancer
- Target Group
![20](https://user-images.githubusercontent.com/52894481/184721287-052bf746-9f6a-4f5b-868e-0fa868e94e3f.JPG)

- Application Load Balancer
![21](https://user-images.githubusercontent.com/52894481/184721324-ed516e2d-39d8-4cdd-8d90-62aff7563490.JPG)

- Copy the Endpoint of the load balancer and create cname record @ godaddy dns 
![godaddy](https://user-images.githubusercontent.com/52894481/184721388-19cf2715-6479-49d9-919c-8eb454bad3f4.JPG)

Step 22 - Create Auto Scaling Group for the EC2 instance.

- Create image from the tomcat instance
- ![23](https://user-images.githubusercontent.com/52894481/184721442-e2452de1-a309-4457-9cf1-145b36e2072d.JPG)

- Create launch configuration
  ![24](https://user-images.githubusercontent.com/52894481/184721483-922d31c7-89bd-4b64-99ac-d6321ca3386a.JPG)

- Create Auto Scaling Group
  ![25](https://user-images.githubusercontent.com/52894481/184721549-7f24095f-a9c1-462b-838a-8fcf0207c0c6.JPG)

Step 23 - Launch Website
![22](https://user-images.githubusercontent.com/52894481/184721598-38930cc0-6b86-4577-9024-612ccf119486.JPG)
