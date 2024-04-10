pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('Dockerhub')
	}

	stages {
           stage("initial Cleanup") {
		steps {
			dir("${WORKSPACE}") {
				deleteDir()
		           }
                     }
                  }


	    stage("checkout git") {
			steps {
			   git branch: 'main', credentialsId: 'Github-id' , url:'https://github.com/babslekson/todo_php.git'
			   }
			}
	    stage ("Building Docker image") {
		steps {
			script	{
				sh ' docker build -t babslekson/php_todo:${BRANCH_NAME}-${BUILD_NUMBER} .'

			} 
		 }	

	     }	
        
        stage ("push image to Docker hub") {
			steps {
			   script {
				    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				    sh 'docker push babslekson/php_todo:${BRANCH_NAME}-${BUILD_NUMBER}'
				}
			     }
	     }
	    
            stage ('Cleanup') {
			steps {
				cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
				
				sh 'docker logout'

				sh 'docker system prune -f'
			}
		}
  	}
}
