pipeline {
    agent any
    
environment {
        GIT_URL = 'https://github.com/boballertech/Project1.git'
        TOMCAT_URL    = 'http://18.117.177.10:8080/'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git "${GIT_URL}"
            }
        }
        stage("SonarQube analysis") {
            steps {
              withSonarQubeEnv('sonar') {
                sh 'cd SampleWebApp && mvn clean package sonar:sonar'
              }
            }
          }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
       stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package -Dbuild.number=${BUILD_NUMBER}'
            }
        } 
        stage('Deploy to jfrog') {
            steps {
                rtUpload (
                    serverId: 'my-jfrog',
                    spec: '''{
                          "files": [
                            {
                              "pattern": "**/*.war",
                              "target": "Demo-repo/"
                            }
                        ]
                    }''',
                )
            }
        }
        stage('Deploy to tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatID', path: '', url: "${TOMCAT_URL}")], contextPath: 'webapp2', war: '**/*.war'
            }
        }
    }
}
