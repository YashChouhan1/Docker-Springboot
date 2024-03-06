pipeline {
    agent any
   
    parameters {
        string(name: 'DOCKER_HUB_USER', defaultValue: 'yashchouhan', description: 'Insert docker hub username')
        string(name: 'DOCKER_HUB_PASSWORD', defaultValue: '', description: 'Insert docker hub password')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    sh "echo 'In checkout stage'"

                    sh "echo 'checking out the code using scm'"
                }
            }
        }


        stage('Build') {
            steps {
                // Build your application here (e.g., compile, package, etc.)
                sh '''
                ls
                echo "In Build Stage"
                '''
            }
        }

        
        stage('Deploy') {
            steps {
                // Deploy your application to a target environment (e.g., staging, production)
                sh "echo 'In deploy stage'"
                
                sh "docker login -u ${params.DOCKER_HUB_USER} -p ${params.DOCKER_HUB_PASSWORD}"        
                            
                echo "creating backend image"
                sh 'docker build -f ./Dockerfile -t yashchouhan/backend-nv1 .'
            
                echo "creating frontend image"
                sh 'docker build -f ./Dockerfile-frontend -t yashchouhan/frontend-nv1 .'

                echo "current images -"
                sh 'docker images'

                echo "pushing backend image"
                sh 'docker push yashchouhan/backend-nv1'

                echo "pushing frontend image"
                sh 'docker push yashchouhan/frontend-nv1'
            }
        } 
        
        stage('Release'){
            steps {
                // Update docker-compose.yml with new image tags
                sh "echo 'In release stage'" 

                sh 'docker-compose -f docker-compose.yml up'
            }
        }   
        
    }

    post {
        success {
            // Actions to perform when the pipeline succeeds
            echo 'Pipeline succeeded!'
        }
        failure {
            // Actions to perform when the pipeline fails
            echo 'Pipeline failed!'
        }
    }
}
