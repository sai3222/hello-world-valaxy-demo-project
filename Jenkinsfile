pipeline {
    agent any
    stages {
        stage ('git checkout') {
            steps {
              git branch: 'master', url: 'https://github.com/sai3222/hello-world-valaxy-demo-project.git'
            }
        }    
        stage ('unit testing') {
            steps {
                sh 'mvn test'
            }
        }    
        stage ('Integration Testing') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }    
        stage ('Maven Build') {
            steps {
                sh 'mvn clean install'
            }    
        }
        stage ('SonarQube-analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      sh 'mvn clean package sonar:sonar'    
                }
                }    
            }
        }
      stage ('Quality Gate status') {
            steps {
                script {
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }    
            }
      }
      stage ('Upload jar nexus') {
          steps {
              script {
                nexusArtifactUploader artifacts: 
                [
                     [
                            artifactId: 'webapp', 
                            classifier: '', file: 'webapp/target/*.war',
                            type: 'war'
                            ]
                ], 
                credentialsId: 'nexus-auth', 
                groupId: 'com.example', 
                nexusUrl: '3.19.66.129:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'demo-app-release/', 
                version: '1.0.0'  
              }
          }
      }    
   }     
}
