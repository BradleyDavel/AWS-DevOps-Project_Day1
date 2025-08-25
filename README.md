<!-- Badges -->
![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws&logoColor=white)
![Java](https://img.shields.io/badge/Java-17-blue?logo=java&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-Build-red?logo=apachemaven&logoColor=white)
![VS Code](https://img.shields.io/badge/Editor-VS%20Code-0078d7?logo=visualstudiocode&logoColor=white)
![DevOps](https://img.shields.io/badge/DevOps-Learning-success?logo=githubactions&logoColor=white)


<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up a Web App in the Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-vscode)

**Author:** Bradley Davel  
**Email:** bradleydavel123@gmail.com

---

## Set Up a Web App Using AWS and VS Code

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_7a1de541)

---

## Introducing Today's Project!

In this project, I will demonstrate how to set up an EC2 instance on AWS,connect to it securely using SSH, and scaffold a simple Java web application with Maven inside VS Code. I’m doing this project to learn the fundamentals of DevOps workflows: provisioning compute, managing SSH keys, configuring security groups, working with VS Code’s Remote-SSH, and building a Java web app end-to-end.

### Key tools and concepts

Services I used were Amazon EC2, VPC, Security Groups, IAM key pairs, and Amazon Linux. Key concepts I learnt include secure SSH access (keybased auth), configuring least-privilege inbound rules, using VS Code Remote-SSH to work directly on the server, and building a Java web app with Maven (POM, dependencies, standard project layout).

### Project reflection

One thing I didn’t expect in this project was how strict SSH private keypermissions are (chmod 400 is mandatory) and how a single misconfiguration in the security group blocks SSH completely—great reminder of least-privilege networking.

This project took me approximately 30 minutes end-to-end. The most challenging part was getting Remote-SSH to use the correct IdentityFile and HostName. It was most rewarding to see the Maven webapp scaffold build and run cleanly on the EC2 instance.


I'll be working on the next project today as well to automate builds and deployments.

---

## Launching an EC2 instance

I started this project by launching an EC2 instance because it provides an ondemand virtual server where I have full control over the operating system,
networking, and software stack. It’s the most straightforward way to practice
real-world DevOps skills like configuring security groups, using SSH for remote
access, and deploying applications in a cloud environment.

### I also enabled SSH

SSH is a secure protocol for encrypted remote login and command
execution. I enabled SSH so that I can administer the EC2 instance safely from my laptop without passwords, using my private key for authentication

### Key pairs

A key pair is a public/private cryptographic key set used for SSH
authentication. AWS stores the public key on the instance and I keep the private .pem file locally to prove my identity when connecting.


Once I set up my key pair, AWS automatically downloaded a single-use .pem file containing my private key. I stored it securely, set its permissions to read-only (chmod 400), and used it for SSH.


---

## Set up VS Code

VS Code is a fast, extensible code editor that supports remote development, terminals, debugging, and plugins like Remote-SSH for editing files directly
on servers.


I installed VS Code to write and edit code, run terminals, and use Remote-SSH to open the EC2 filesystem as a workspace so I can build and manage the app as if it were local.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_53d05e68)

---

## My first terminal commands

A terminal is a text-based interface to run commands on a computer or server. The first command I ran for this project was: cd C:\Users\bradl\Desktop\DevOps


Then I changed file permissions using: icacls
"nextwork-keypair.pem" /reset icacls
"nextwork-keypair.pem" /grant:r "bradl:R"
icacls "nextwork-keypair.pem" /inheritance:r

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_9328ada1)

---

## SSH connection to EC2 instance

To connect to my EC2 instance, I ran: ssh -i nextworkkeypair.pem ec2-user@ec2-xx-xx-xx-xx.compute1.amazonaws.com (Replace with your instance’s
Public IPv4 DNS.)

### This command required an IPv4 address

A server’s IPv4 DNS is the public hostname (e.g., ec2-…compute1.amazonaws.com) that resolves to the instance’s public IPv4 address, so I don’t need to remember the raw IP.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_e3069dca)

---

## Maven & Java

Apache Maven is a build and dependency-management tool for Java that uses a pom.xml to define project metadata, dependencies, plugins, and standardized build lifecycles.


Maven is required in this project because it quickly scaffolds a standard Java webapp, manages dependencies consistently, and provides a repeatable build process across environments.


Java is a general-purpose, object-oriented programming language with strong tooling, portability (JVM), and a large ecosystem for building web applications
and services.


Java is required in this project because the lab targets a simple Java web application; it’s widely used in enterprise backends and deploys cleanly on common web containers.

---

## Create the Application

To do this, use these
commands: 

mvn archetype:generate \
-DgroupId=com.nextwork.app \
-DartifactId=nextwork-web-project \
-DarchetypeArtifactId=maven-archetype-webapp \
-DinteractiveMode=false

When you run mvn commands, you're asking Maven to perform tasks (like creating a new project or building an existing one). The mvn archetype:generate command specifically tells Maven to create a new project from a template (which Maven calls an archetype). This command sets up a basic structure for your project, so you don't have to start from scratch.

I installed Remote-SSH so VS Code can open the EC2 instance as a remote workspace, letting me edit files, run builds, and manage the app directly on the server over an encrypted SSH connection.

Configuration details required to set up a remote connection (in ~/.ssh/config) include:

Host aws-ec2
HostName <your-public-IPv4-DNS>
User ec2-user
IdentityFile /path/to/bradley-keypair.pem

Then I can run: ssh aws-ec2

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_2939cf01)

---

## Create the Application

Using VS Code’s file explorer, I could see a standard Maven layout: pom.xml, the src directory, and src/main/webapp (with WEB-INF) for web resources/JSPs.

The src folder holds Java source code and resources; the webapp folder contains web assets and JSP pages. WEB-INF under webapp stores deployment descriptors and non-public resources.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_45f91fd7)

---

## Using Remote - SSH

The index.jsp is the default landing JSP for the application, rendered by the Java web container when I access the app’s root URL

I opened `index.jsp` in VS Code via Remote-SSH, edited the HTML (e.g., change the `<h1>` message), saved the file, and rebuilt/redeployed the app with Maven as needed.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-devops-vscode_7a1de541)

