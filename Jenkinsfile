pipeline {
    agent any
    
environment {
        GIT_URL = 'https://github.com/boballertech/Project1.git'
        TOMCAT_URL    = 'http://18.117.82.42:8080/'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git "${GIT_URL}"
            }
        }
       stage('build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }
        stage('deploy to tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatID', path: '', url: "${TOMCAT_URL}")], contextPath: 'webapp2', war: '**/*.war'
            }
        }
    }
}
