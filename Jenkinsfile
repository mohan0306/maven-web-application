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
        REMOTE_TOMCAT_DIR = '/mnt/apache-tomcat-9.0.91/webapps'
        PEM_KEY_PATH = '/home/jenkins/Linux_DevOps.pem'  // Path to PEM key on Jenkins server
        S3_BUCKET_NAME = 'maven-buid-version'
        AWS_REGION = 'ap-south-1'
    }
    
    stages {
        stage('Continuous Download') {
            steps {
                echo 'Downloading source code from GitHub'
                git env.GIT_REPO_URL
                echo 'Source code downloaded successfully.'
            }
        }

        stage('Continuous Build') {
            steps {
                echo 'Checking version of Maven'
                sh 'mvn --version'
                echo 'Starting build process using Maven'
                sh 'mvn clean install'
                echo 'Build process successfully completed'
            }
        }

        stage('Continuous Upload/Backup') {
            steps {
                echo 'Uploading and backing up the WAR file locally and to the backup repository'

                // Deploy to local Tomcat
                sh "cp ${WAR_FILE_PATH} ${LOCAL_TOMCAT_DIR}"
                echo 'Build successfully deployed to local Tomcat'

                // Backup WAR file
                sh "cp ${WAR_FILE_PATH} ${BACKUP_DIR}"
                echo 'Build successfully uploaded/backed up'
            }
        }

        stage('Upload to S3') {
            steps {
                echo 'Uploading build artifact to S3'

                // Upload WAR file to S3
                sh """
                    aws s3 cp ${WAR_FILE_PATH} s3://${S3_BUCKET_NAME}/maven-web-application-#${currentBuild.number}.war --region ${AWS_REGION}
                """

                echo 'Build artifact successfully uploaded to S3'
            }
        }

        stage('Continuous Delivery on Webserver') {
            steps {
                echo 'Starting production deployment'

                // Use PEM key for SCP transfer to the remote server
                sh """
                    scp -i ${PEM_KEY_PATH} -o StrictHostKeyChecking=no ${WAR_FILE_PATH} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_TOMCAT_DIR}
                """

                echo 'Current build is deployed to production successfully'
            }
        }

        stage('Restart Tomcat') {
            steps {
                echo 'Restarting Tomcat on the remote server'

                // Restart Tomcat on the remote server
                sh """
                    ssh -i ${PEM_KEY_PATH} -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} 'sudo /etc/init.d/tomcat restart'
                """


                echo 'Tomcat successfully restarted'
            }
        }
    }
}
