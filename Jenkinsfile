
pipeline {
	agent any
	tools {
            maven 'M2_HOME'
        }
        environment {
            		registry = "amineazri/achatproject"
            		registryCredential = 'dockerHub'
            		dockerImage = ''
            	}
	stages{
		stage('Cloning Code){
            steps{
                echo 'Pulling...';
                git branch: 'main',
                url :'https://github.com/Azriiii/Devops.git';
            }			
        }
		
		stage('Build'){
            steps{
                sh 'mvn clean install'
            }			
        }
		
		stage('Compile'){
            steps{
                sh 'mvn compile'
            }	
    	}
			
		stage('SonarQube Analysis'){
            steps{
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=amine'
            }			
        }
        stage('Test: Mocktio') {
                                    steps {
                                   sh 'mvn clean test -Dtest=com.esprit.examen.services.ProduitServiceMockTest'
                                    }
                                }
                                 stage('Test: Junit') {
                                    steps {
                                    sh 'mvn test'
                                    }
                                }
        stage('Nexus'){
                                    steps{
                                        sh 'mvn deploy -DskipStaging=true '
                                    }

                                }
	stage('Starting Container') {
                 steps {
                    sh  'docker-compose -v'
                    sh 'docker-compose up -d '
                    sh 'docker-compose ps'
          }
            }
	stage('Building Docker Image') {
                steps {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }
            stage('Deploying Docker Image') {
                        steps {
                            script {
                                docker.withRegistry( '', registryCredential ) {
                                    dockerImage.push()
                                }
                            }
                        }
                    }

stage('Post Actions : Mailing') {

            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

}
}
post {
        always {
            echo 'This will always run'
        }
        success {
            mail to: "amine.azri@esprit.tn",
                     subject: "Success",
                     body: "Succes on job ${env.JOB_NAME}, Build Number: ${env.BUILD_NUMBER} "
        }
        failure {
                    mail to: "amine.azri@esprit.tn",
                     subject: "Failure",
                     body: "Failure on job ${env.JOB_NAME}, Build Number: ${env.BUILD_NUMBER}, Build URL: ${env.BUILD_URL} "
                }


    }
}