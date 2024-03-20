## What this does?
This repo along with https://github.com/saha-rajdeep/kubernetesmanifest creates a Jenkins pipeline with GitOps to deploy code into a Kubernetes cluster. CI part is done via Jenkins and CD part via ArgoCD (GitOps).

## Jenkins installation
Jenkins is installed on EC2. Follow the instructions on https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/ . You can skip "Configure a Cloud" part for this demo. Please note some commands from this link might give errors, below are the workarounds:

1. If you get daemonize error while running the command `sudo yum install jenkins java-1.8.0-openjdk-devel -y` then , run the commands from the answer of https://stackoverflow.com/questions/68806741/how-to-fix-yum-update-of-jenkins

2. Install Docker on the EC2 after Jenkins is installed by following the instructions on https://serverfault.com/questions/836198/how-to-install-docker-on-aws-ec2-instance-with-ami-ce-ee-update

3. Run `sudo chmod 666 /var/run/docker.sock` on the EC2 after Docker is installed.

4. Install Git on the EC2 by running `sudo yum install git`

## Jenkins installation - better alternative
Pre-Requisites:

Java (JDK)
Run the below commands to install Java and Jenkins
Install Java
'''
sudo apt update
sudo apt install openjdk-11-jre
'''
Verify Java is Installed
'''
java -version
'''
Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

EC2 > Instances > Click on
In the bottom tabs -> Click on Security
Security groups
Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed All traffic).
![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/c8188ee1-d943-44fe-8938-566cef7cc70f)


Login to Jenkins using the below URL:
http://:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing 'All Traffic' to your EC2 instance 1. Delete the inbound traffic rule for your instance 2. Edit the inbound traffic rule to only allow custom TCP port '8080'

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - 'sudo cat /var/lib/jenkins/secrets/initialAdminPassword' - Enter the Administrator password

![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/3707d11a-761f-4e0a-8f3e-9e3792aebcb2)


Click on Install suggested plugins

![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/6e2ddea1-b4d1-4cf7-9021-89e518c58fc6)


Wait for the Jenkins to Install suggested plugins


![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/ca7053fc-4c89-49a4-9925-3efc1f9275d4)


Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/7b47b27b-a4c6-4634-a1e9-14700a77fd17)


Jenkins Installation is Successful. You can now starting using the Jenkins

![image](https://github.com/phibiam/Continuous-Int_Deploy/assets/24440690/211202e2-a4bf-4a37-bd5d-7995888fd044)


### Jenkins plugins

Install the following plugins for the demo.
- Amazon EC2 plugin (No need to set up Configure Cloud after)
- Docker plugin  
- Docker Pipeline
- GitHub Integration Plugin
- Parameterized trigger Plugin

## ArgoCD installation 

Install ArgoCD in your Kubernetes cluster following this link - https://argo-cd.readthedocs.io/en/stable/getting_started/

## How to run!
Follow along with my Udemy Kubernetes course lectures (GitOps Chapter) to understand how it works, detailed setup instructions, with step by step demo. My highest rated Kubernetes EKS discounted Udemy course link in www.cloudwithraj.com
