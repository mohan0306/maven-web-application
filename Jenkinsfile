
currentBuild.displayName = "maven-web-application-#"+currentBuild.number
pipeline{
    agent any    
    stages {
        stage('continuous download') {
            steps {
                git 'https://github.com/mohan0306/maven-web-application.git'
                echo 'Source code downloaded successfully'
            }
               
        }
        stage('continuous build') {
            steps {
                sh 'mvn clean install'
                echo 'Build process successfully completed'
            }
        }
            stage('continuous upload/backup') {
            steps {
                echo 'Uploading and backing up the WAR file at local and maven repository'
                sh 'cp /root/.jenkins/workspace/Java_project/target/maven-web-application.war /root/apache-tomcat-9.0.91/webapps'
                echo 'Build successfull deployed at local tomcat'
                sh 'cp /root/.jenkins/workspace/Java_project/target/maven-web-application.war /mnt/Backup_snapshot'
                echo 'Build successfull uploaded/backup'
            }
               
        }
        stage('continuous Deliver on Webserver') {
            steps {
                echo 'production deployment'
                sh 'ssh ec2-user@172.31.24.48'
                echo 'ssh connection established successfull'
                sh 'scp /root/.jenkins/workspace/Java_project/target/maven-web-application.war ec2-user@172.31.24.48:/home/ec2-user/apache-tomcat-9.0.91/webapps'
                echo 'current build is deployed to production'
            }
        } 
}
   
}
