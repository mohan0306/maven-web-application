// Set the display name of the current build
currentBuild.displayName = "maven-web-application-#" + currentBuild.number

pipeline {
    agent any    

    // Define environment variables for easy configuration
    environment {
        GIT_REPO_URL = 'https://github.com/mohan0306/maven-web-application.git'
        WAR_FILE_PATH = '/root/.jenkins/workspace/Java_project/target/maven-web-application.war'
        LOCAL_TOMCAT_DIR = '/root/apache-tomcat-9.0.91/webapps'
        BACKUP_DIR = '/mnt/Backup_snapshot'
        REMOTE_USER = 'ec2-user'
        REMOTE_HOST = '172.31.24.48'
        REMOTE_TOMCAT_DIR = '/home/ec2-user/apache-tomcat-9.0.91/webapps'
        PEM_KEY_PATH = '/home/jenkins/.ssh/my-key.pem'  // Path to PEM key on Jenkins server
    }
    
    stages {
        stage('Continuous Download') {
            steps {
                echo 'Downloading source code from GitHub...'
                git env.GIT_REPO_URL
                echo 'Source code downloaded successfully.'
            }
        }

        stage('Continuous Build') {
            steps {
                echo 'Starting build process using Maven...'
                sh 'mvn clean install'
                echo 'Build process successfully completed.'
            }
        }

        stage('Continuous Upload/Backup') {
            steps {
                echo 'Uploading and backing up the WAR file locally and to the backup repository...'

                // Deploy to local Tomcat
                sh "cp ${WAR_FILE_PATH} ${LOCAL_TOMCAT_DIR}"
                echo 'Build successfully deployed to local Tomcat.'

                // Backup WAR file
                sh "cp ${WAR_FILE_PATH} ${BACKUP_DIR}"
                echo 'Build successfully uploaded/backed up.'
            }
        }

        stage('Continuous Delivery on Webserver') {
            steps {
                echo 'Starting production deployment...'

                // Use PEM key for SCP transfer to the remote server
                sh """
                    scp -i ${PEM_KEY_PATH} -o StrictHostKeyChecking=no ${WAR_FILE_PATH} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_TOMCAT_DIR}
                """

                echo 'Current build is deployed to production successfully.'
            }
        }
    }
}
