pipeline{
    
    agent any
	tools{
		maven 'maven'
	} 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Risheekdev/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('sonarqube'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-key') {
                        sh 'mvn clean package sonar:sonar'
                }
            }
        }
        
    }
    stage('Quatity gate checking'){

        steps{

            script{
                waitForQualityGate abortPipeline: false, credentialsId: 'sonar-key'
            }
        }
    }
    stage('upload war to nexus'){

        steps{

            script{

                def readPomVersion = readMavenPom file: 'pom.xml'

                nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-auth', groupId: 'com.example', nexusUrl: '13.233.104.249:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'poc-project', version: "${readPomVersion.version}"
            }
        }
    }
        
  }

}
