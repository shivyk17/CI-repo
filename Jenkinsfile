pipeline {
    agent { label 'Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3' 
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/shivyk17/CI-repo.git'
                }
        }
	    stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
       }
	    stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
	  stage("SonarQube Analysis"){
		environment {
                SONAR_URL = "http://172.31.7.89:9000"
            }  
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'Sonarqube') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
	    stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonarqube'
                }	
            }
        }
    }
}

        
