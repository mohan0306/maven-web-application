
currentBuild.displayName = "maven-web-application-#"+currentBuild.number
pipeline{
    agent any    
    stages {
        stage('continuous download') {
            steps {
                git 'https://github.com/mohan0306/maven-web-application.git'
            }
               
        }
        stage('continuous build') {
            steps {
                sh 'mvn clean install'
            }
        }
            stage('continuous upload/backup') {
            steps {
                sh 'cp /root/.jenkins/workspace/Java_project/target/maven-web-application.war /root/apache-tomcat-9.0.91/webapps'
                sh 'cp /root/.jenkins/workspace/Java_project/target/maven-web-application.war /mnt/Backup_snapshot'
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
