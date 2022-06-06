# projCert
Problem Statement - I

Steps for executing the solution:
• Use the Master VM for Jenkins, Ansible, GIT etc.
• Use the fresh instance for Jenkins Slave Node (Test Server)
• Change the IP address of the VMs accordingly
• Add Build Pipeline Plugin and Post-build task plugin to Jenkins on the master VM
• Install python, openssh-server and git on the slave node manually
• Use the image devopsedu/webapp and add your PHP website to it using a Dockerfile
• Push the PHP website, and the Dockerfile to a git repository
 
 Below tasks are automated through Jenkins by creating a pipeline:
1. Install and configure puppet agent on the slave node (Job 1)
2. Push an Ansible configuration on test server to install docker (Job 2)
3. Pull the PHP website, and the Dockerfile from the git repo and build and deploy your PHP
docker container. After. (Job 3)
4. If Job 3 fails, delete the running container on Test Server
