# Project0

This is designed to be a POC(proof of concept) of Jenkins CI using Docker enabled AWS EC2 environment.

1.Sign up two EC2 instances on AWS, as one Master Node and one Slave Node for Jenkins. Also make sure you open an inbound port for your security group. I open 8080 for Jenkins.

2.Install Docker and Git on two instances.

```
[ec2-user ~]$ sudo yum update -y
[ec2-user ~]$ sudo yum install -y docker
[ec2-user ~]$ sudo service docker start
# Add the ec2-user to the docker group so you can execute Docker commands without using sudo.
[ec2-user ~]$ sudo usermod -a -G docker ec2-user
# Install git.
[ec2-user ~]$ sudo yum install git -y
```

3.Install and run Jenkins from a Docker image.

```
##Pull the official jenkins image from Docker repository.
[ec2-user ~] docker pull jenkins
##Run a container using this image and map data directory from the container to the host. You can make that a persistent volume.
[ec2-user ~]docker run -p 8080:8080 -v /home/ec2-user/jenkins_home:/var/jenkins_home jenkins
```

Visit your jenkins home page http://<your public ip>:8080/
e.g. http://54.191.4.24:8080/

Remember to modify the security rules. Register an account. Then disable sign up. So only you have the access to modify Jenkins configuration.

4.Set up master and slave machines
**There is no need to install Jenkins on the slave machine.**
* On your jenkins homepage go to *Manage Jenkins* --> *Manage Node*
* *New Node* --> *Enter Node Name* Name it as "Slave"
* Select Dumb Slave --> Press *OK*
* Set file system root "/home/ec2/jenkins_home" for example.
* Select *Usage* for *Utilize this slave as much as possible*
* *Launch Method* select *Launch slave agents on Unix machines via SSH". Type in slave public ip as the *Host*. Add the ec2-user private key as login credentials.
* *Availability*: *Keep this slave online as much as possible*
* Press OK

5.Install GitHub plugin on Jenkins.
"Manage Jenkins" --> "Manage Plugins" --> search for "Github Plugin"
Install it and restart the Jenkins.

6.Configure GitHub webhook.
Add "http://<your public id>:8080/github-webhook/" to webhook service in your GitHub repository setting.

7.Test by manually build a job. Or make a push to the GitHub repository.



