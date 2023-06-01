pipeline {
		agent any
		tools{
			maven 'maven-3.8.8'
		}  
		
		environment {
        registry = '579552548120.dkr.ecr.us-east-1.amazonaws.com/container-repodata'
        registryCredential = 'Jenkins-ecr-login-credentials'
        dockerimage = ''
				}
				stages{
			stage("Checkout the project") {
			   steps{
				   git branch: 'master', credentialsId: 'github', url: 'https://github.com/psharma56/springboot-maven-micro.git'
			   } 
			}
			        stage("Build the package"){
            steps {
                sh 'mvn clean package'
		  }
		}
				stage("Sonar Quality Check"){
		steps{
		    script{
		     withSonarQubeEnv(installationName: 'sonar-10', credentialsId: 'jenkins-sonar-token') {
		     sh 'mvn sonar:sonar'
	    	}
			timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
            	}	
            }	
	      }
	    }
	  }
	  stage('Building the Image') {
        steps {
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
	        }
          }
        }
		stage ('Deploy the Image to Amazon ECR') {
       steps {
           script {
           docker.withRegistry("http://" + registry, "ecr:us-east-1:" + registryCredential ) 
		   {
           dockerImage.push()
     }
   }
   }
   }
   }
   }
