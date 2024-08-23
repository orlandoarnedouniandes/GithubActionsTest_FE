pipeline { 
    agent any
    environment {
        HOME = "${WORKSPACE}"
        GIT_REPO = 'MISW4201-202414-Frontend-Grupo02'
        GITHUB_TOKEN_ID = '43771338-0057-4a96-ae03-93ee5419d871'
    }
    stages {
        stage('Checkout') { 
            steps {
                git branch: 'main',  
                    credentialsId: env.GITHUB_TOKEN_ID,
                    url: 'https://github.com/MISW-4201-ProcesosDesarrolloAgil/' + env.GIT_REPO
            }
        }
        stage('Gitinspector') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.GITHUB_TOKEN_ID, passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
	                sh 'mkdir -p code-analyzer-report'
	                sh """ curl --request POST --url https://code-analyzer.virtual.uniandes.edu.co/analyze --header "Content-Type: application/json" --data '{"repo_url":"git@github.com:MISW-4201-ProcesosDesarrolloAgil/${GIT_REPO}.git", "access_token": "${GIT_PASSWORD}" }' > code-analyzer-report/index.html """   
                }  
	            publishHTML (target: [
	                allowMissing: false,
	                alwaysLinkToLastBuild: false,
	                keepAll: true,
	                reportDir: 'code-analyzer-report',
	                reportFiles: 'index.html',
	                reportName: "GitInspector"
                ])
            }
        }
    }
    post { 
      always { 
          cleanWs()
          deleteDir() 
          dir("${env.GIT_REPO}@tmp") {
              deleteDir()
	      }
      }
   }
}
