# Setting up Jenkins on ubuntu 16.04

![Imgur](http://i.imgur.com/dDI8HaN.jpg)

### Introduction

Jenkins is an open source automation server intended to automate repetitive technical tasks involved in the continuous integration and delivery of software. 


> Continuous Integration is a development practice that requires developers to integrate code into a shared repository at regular intervals. This concept was meant to remove the problem of finding later occurrence of issues in the build lifecycle. Continuous integration requires the developers to have frequent builds. The common practice is that whenever a code commit occurs, a build should be triggered.


Jenkins is Java-based and can be installed from Ubuntu packages or by downloading and running its Web application ARchive (WAR) file — a collection of files that make up a complete web application which is intended to be run on a server.

Jenkins will be installed on a server where the central build will take place. The following flowchart demonstrates a very simple workflow of how Jenkins works.

![Imgur](http://i.imgur.com/SjQSHoU.jpg)


### Prerequisites

Before proceeding further, you mush have a server on which we are going to install jenkins. If you do not have a server right now, follow this guide before proceeding further.

Its recommended to start with atleast 1GB of RAM. When the server is set up, you are ready to follow along.


# Installing Jenkins

The version of Jenkins included with the default Ubuntu packages is often behind the latest available version from the project itself. In order to take advantage of the latest fixes and features, we'll use the project-maintained packages to install Jenkins.

- First, we'll add the repository key to the system.

	`wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -`

- When the key is added, the system will return OK. Next, we'll append the Debian package repository address to the server's `sources.list`:

	`echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list`
		
- When both of these are in place, we'll run `update` so that `apt-get` will use the new repository:

	`sudo apt-get update`
	
- Finally, we'll install Jenkins and its dependencies, including Java:

	`sudo apt-get install jenkins`
	
Now that Jenkins and its dependencies are in place, we'll start the Jenkins server.

# Starting Jenkins Server

Using `systemctl` we'll start Jenkins :

	sudo systemctl start jenkins
	
Since `systemctl` doesn't display output, we'll use its status command to verify that it started successfully

	sudo systemctl status jenkins

If everything went well, the beginning of the output should show that the service is active and configured to start at boot :

	 ● jenkins.service - LSB: Start Jenkins at boot time
     Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
     Active: active (exited) since Thu 2017-08-10 09:38:44 UTC; 6 days ago
     Docs: man:systemd-sysv-generator(8)
     Process: 1422 ExecStart=/etc/init.d/jenkins start (code=exited, status=0/SUCCESS)
     Tasks: 0
     Memory: 0B
     CPU: 0

Now that Jenkins is running, we'll adjust our firewall rules so that we can reach Jenkins from a web browser to complete the initial set up.

# Opening the firewall

By default, Jenkins runs on port `8080`, so we'll open that port using ufw:

	sudo ufw allow 8080

We can see the new rules by checking UFW's status.

	sudo ufw status

We should see that traffic is allowed to port 8080 from anywhere:

	Status: active
	
	To                         Action      From
	--                         ------      ----
	OpenSSH                    ALLOW       Anywhere
	8080                       ALLOW       Anywhere
	OpenSSH (v6)               ALLOW       Anywhere (v6)
	8080 (v6)                  ALLOW       Anywhere (v6)
	
Now that Jenkins is installed and the firewall allows us to access it, we can complete the initial setup.

# Setting up Jenkins

To set up our installation, we'll visit Jenkins on its default port, `8080`, using the server domain name or IP address: 
	
	http://<ip_address_or_domain_name>:8080

We should see "Unlock Jenkins" screen, which displays the location of the initial password 

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/unlock-jenkins.png)

In the terminal window, we'll use the `cat` command to display the password:

	sudo cat /var/lib/jenkins/secrets/initialAdminPassword
	
We'll copy the 32-character alphanumeric password from the terminal and paste it into the "Administrator password" field, then click "Continue". The next screen presents the option of installing suggested plugins or selecting specific plugins.

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-customize.png)

We'll click the "Install suggested plugins" option, which will immediately begin the installation process:

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-plugins.png)

When the installation is complete, we'll be prompted to set up the first administrative user. It's possible to skip this step and continue as admin using the initial password we used above, but we'll take a moment to create the user.

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-first-admin.png)

Once the first admin user is in place, you should see a "Jenkins is ready!" confirmation screen.

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-ready.png)

Click "Start using Jenkins" to visit the main Jenkins dashboard:

![](https://assets.digitalocean.com/articles/jenkins-install-ubuntu-1604/jenkins-using.png)

At this point, Jenkins has been successfully installed.

# Conclusion

In this tutorial, we've installed Jenkins using the project-provided packages, started the server, opened the firewall, and created an administrative user. At this point, you can start exploring Jenkins.
