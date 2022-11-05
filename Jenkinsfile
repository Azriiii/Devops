
pipeline {
	agent any 
	stages{
		stage('Checkout Git'){
            steps{
                echo 'Pulling...';
                git branch: 'main',
                url :'https://github.com/Azriiii/Devops.git';
            }			
        }
		
		stage('MVN CLEAN'){
            steps{
                sh 'mvn clean'
            }			
        }
		
		stage('MVN COMPILE'){
            steps{
                sh 'mvn compile'
            }	
    	}
			
		stage('MVN SONARQUBE'){
            steps{
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=amine'
            }			
        }
        stage('MOCKITO') {
                                    steps {
                                   sh 'mvn clean test -Dtest=com.esprit.examen.services.ProduitServiceMockTest'
                                    }
                                }
                                 stage('JUNIT') {
                                    steps {
                                    sh 'mvn test'
                                    }
                                }
        stage('NEXUS'){
                                    steps{
                                        sh 'mvn deploy -DskipStaging=true '
                                    }

                                }
	}
}
