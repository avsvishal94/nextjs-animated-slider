pipeline {
agent any
    parameters {
        choice(
            choices: ['Yes', 'No', 'Not so sure'],
            description: 'Clean workspace before checkout?',
            name: 'CLEAN_WORKSPACE'
        )
    }
    stages {
        stage('Clean Workspace') {
             when {
                expression { params.CLEAN_WORKSPACE == 'Yes' }
            }
            steps {
                cleanWs()
             }
        }
  		 stage('Checkout') {
    		 steps {
      			 git branch: 'main', url: 'https://github.com/avsvishal94/nextjs-animated-slider.git'
    		 }
  		 }
  		stage('Validate the installation') {
    		steps {
      			sh '''
                bash auto.sh
            '''
    		}
  		}
	}
	post {
		always { 
            echo 'The pipeline has been initiated, check for the status in email!'
    }
  		success {
    		notify ('SUCCESS','good')
  		}
  		failure {
    		notify ('FAIL','#FF0000')
  		}
	}
}

def notify(status,colour){
	emailext (
		to: 'avsvishal94@gmail.com',
		subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
		body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'</p>
		<p>Check the console output	<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>"""
	)
}
