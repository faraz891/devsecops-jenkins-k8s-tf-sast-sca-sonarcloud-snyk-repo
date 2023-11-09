pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=systemops-de -Dsonar.organization=systemops-de -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=d9041166a3325343a80a9553441c9f11993a4703'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test'
				}
			}
    }		
  }
}
