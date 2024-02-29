pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{

	stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=systemops-de -Dsonar.organization=systemops-de -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=43b1b891b222bc095079f5e1b4486d2da0f7d2a0'
			}
    }   
    
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	   stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	   stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://176422384401.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}

	   	   
	   
  }
}
