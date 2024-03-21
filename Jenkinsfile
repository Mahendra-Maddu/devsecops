pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp -Dsonar.organization=asgbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=9325586a8f1d1adf470b908a46156f5844'
			}
        } 
      stage('RunSCAAnalysisUsingSnyk') {
            steps {		
                         withCredentials([string(credentialsId: 'SNYK_TOKEN',variable: 'SNYK_TOKEN')]){
                              sh 'mvn snyk:test -fn'
                         }
			}
  }
      stage('Build'){
            steps {
                  withDockerRegistery([credentialsId: "dockerlogin",url: ""]) {
                        script{
                              app = docker.build("asg")
                        }
                  }
            }
      }
      stage('Push'){
            steps {
                  script {
                        docker.withRegistery('AWS ECR URL','ecr:us-west-2:aws-credentials'){
                              app.push("latest")
                        }
                  }
            }
      }
}
