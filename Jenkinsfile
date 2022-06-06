pipeline {
	agent any
	    stages {
	        stage('install puppet on slave') {
	            agent { label 'Slave 1'}
	            steps {
	                echo 'Install Puppet'
	                sh "sudo yum install -y wget"
	                sh "wget https://apt.puppetlabs.com/puppet-release-bionic.deb"
	                sh "sudo dpkg -i puppet-release-bionic.deb"
	                sh "sudo yum update"
	                sh "sudo yum install -y puppet-agent"
	            }
	        }
	

	        stage('configure and start puppet') {
	            agent { label 'Slave 1'}
	            steps {
	                echo 'configure puppet'
	                sh "mkdir -p /etc/puppetlabs/puppet"
	                sh "if [ -f /etc/puppetlabs/puppet/puppet.conf ]; then sudo rm -f /etc/puppetlabs/puppet.conf; fi"
	                sh "echo '[main]\ncertname = node1.local\nserver = puppet' >> ~/puppet.conf"
	                sh "sudo mv ~/puppet.conf /etc/puppetlabs/puppet/puppet.conf"
	                echo 'start puppet'
	                sh "sudo systemctl start puppet"
	                sh "sudo systemctl enable puppet"
	            }
	        }
	

	

	        stage('Sign in puppet certificate') {
	            agent{ label 'jenkins'}
	            steps {
	              catchError {
	                sh "sudo /opt/puppetlabs/bin/puppet cert sign node1.local"
	              }
	            }
	        }
	

	

	        stage('Install Docker-CE on slave through puppet') {
	            agent{ label 'jenkins'}
	            steps {
	                sh "sudo /opt/puppetlabs/bin/puppet apply /BeverleyAdeyeye/projCert"
	            }
	        }
	

	        stage('Git Checkout') {
	            agent{ label 'Slave 1'}
	            steps {
	                sh "if [ ! -d '/BeverleyAdeyeye/projCert ]; then git clone https://github.com/BeverleyAdeyeye/projCert.git /BeverleyAdeyeye/projCert; fi"
	                sh "cd /BeverleyAdeyeye/projCert && git checkout master"
	            }
	        }
	

	        stage('Docker Build and Run') {
	            agent{ label 'Slave 1'}
	            steps {
	                sh "sudo docker rm -f webapp || true"
	                sh "cd /BeverleyAdeyeye/projCert && sudo docker build -t test ."
	                sh "sudo docker run -it -d --name webapp -p 8081:80 test"
	            }
	        }
	

	        stage('Check if selenium test run') {
	            agent{ label 'Slave 1'}
	            steps {
	                sh "cd /BeverleyAdeyeye/projCert && java -jar devops-webapp.jar"
	            }
	            post {
	                failure {
	                    sh "sudo docker rm -f webapp"
	                }
	            }
	        }
	    }
	}
