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
                    withSonarQubeEnv(credentialsId: 'sonar-auth') {
                        sh 'mvn clean package sonar:sonar'
                }
            }
        }
        
    }
        
}
