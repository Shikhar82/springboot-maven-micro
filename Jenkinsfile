	pipeline {
		agent any
		tools{
			maven 'maven-3.8.8'
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
	}	
	}
