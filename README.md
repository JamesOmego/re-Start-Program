# Setting up AWS EC2 instance
1. Log into AWS console
2. EC2 > launch instance
3. Choose a name
4. Select Ubuntu 22.04 operating system
5. Choose instance type that is the minimum required for the project
6. Select key pair, or create one
7. Allow SSH traffic from your computer IP address only
8. Select the amount of EBS storage required
9. Launch instance
10. Go to instance details > security > security groups > inbound > add rule
11. Add the following custom TCP rules: port 8787 (rstudio), port 8888 (jupyterlab)
12. Copy the IP address
13. Log in via ssh: ssh -i <key> ubuntu@<ip>
14. Run an OS update:
```
sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade
sudo reboot
```
15. Log back in once rebooted and clone this repository: git clone https://github.com/stuart-lab/aws-setup.git
16. Run startup script to install dependencies: sh aws-setup/startup.sh
17. Logout
# Installing AWS CLI
```
Configure:
```
aws configure
To create AWS access keys, log into the AWS console and go to:

Security credentials -> Access keys -> Create new access key

Note the key ID and secret access key.

Storing logs
A*STAR policy requires that system logs are stored for a minimum of 1 year for EC2 instances. To ensure logs are stored, we copy from ``/var/log/`` to an S3 bucket using a shell script. This shell script can be run automatically each time you log out of the server by including it in the ~/.bash_logout file.

First, make sure the aws cli is authenticated so that you can write to the S3 bucket (above). Next, add this code to ``~/.bash_logout`` to ensure compliance with A*STAR policies:

#copy logs to S3 bucket for storage
```
aws s3 cp /var/log/ s3://marcuslab-logs/$(date +'%d_%m_%Y')/$RANDOM --recursive --exclude "*" --include "*log"
```
