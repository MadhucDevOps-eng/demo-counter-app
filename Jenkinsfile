pipeline{
    
    agent any

     environment {
        registry = "910589273559.dkr.ecr.ap-south-1.amazonaws.com/myspringboot-repo"
    }

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
                def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "poc-snapshot" : "poc-project"

                nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'nexus-auth', groupId: 'com.example', nexusUrl: '3.7.71.102:8081', nexusVersion: 'nexus3', protocol: 'http', repository: nexusRepo, version: "${readPomVersion.version}"
            }
        }
    }
    stage('Docker image Build'){
        steps{

            script{

                sh 'docker build -t myspringboot-repo .'
                sh 'docker tag myspringboot-repo:latest 910589273559.dkr.ecr.ap-south-1.amazonaws.com/myspringboot-repo:latest'
                


            }
        }
    }
    stage('push image to dockerHub'){

        steps{

            script{


                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 910589273559.dkr.ecr.ap-south-1.amazonaws.com'
                    sh 'docker push 910589273559.dkr.ecr.ap-south-1.amazonaws.com/myspringboot-repo:latest'
                


            }
        }
    }

        
  }

}
