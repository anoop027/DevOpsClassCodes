pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MyMaven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/anoop027/example-tomcat-war'
                sh "mvn  clean package"
            }
        }
    
        stage('connect to ansible server') {
            steps {
                sshagent(['ansible-user']) {
                   sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Java-Tomcat-project1/target/SimpleTomcatWebApp.war ec2-user@172.31.17.178:/artifact"
                   
                }
            }
        }   
        stage('run ansible file') {
        agent {label 'ansib1'}
            steps {
                ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'ansible-key', installation: 'Ansible1', inventory: '/etc/ansible/hosts', playbook: '/artifact/copy-artifact.yml'
                }
            }
        }
}
