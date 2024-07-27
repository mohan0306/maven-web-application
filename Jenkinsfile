
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
                sh 'ssh root@172.31.94.211'
                 sh 'scp /root/.jenkins/workspace/maven-web-app-deploy/target/maven-web-application.war root@172.31.94.211:/mnt/apache-tomcat-9.0.78/webapps'
            }
        } 
}
   
}
