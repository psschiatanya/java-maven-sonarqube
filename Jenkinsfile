   pipeline {
    agent any

    tools {
        // Specify the JDK and Maven versions installed on the Jenkins server
        jdk 'JDK17'
        maven 'Maven3'
		}

    environment {
        // Set any environment variables needed, e.g., JAVA_HOME, MAVEN_HOME, SONARQUBE
        JAVA_HOME = "${tool 'JDK17'}"
        MAVEN_HOME = "${tool 'Maven3'}"
		
        PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
        SONAR_TOKEN = credentials('sonarqube-token') 
		SONARQUBE_SERVER = 'sonarqube'
		
		
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the repository
                git url: 'https://github.com/psschiatanya/java-maven-sonarqube', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                // Clean and compile the project
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
            post {
                always {
                    // Archive test results
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
		
 /*		stage('Code Analysis') {
            steps {
                script {
                    // Run SonarQube scan
                    
                        sh '''mvn clean verify sonar:sonar  -Dsonar.java.binaries=. -Dsonar.projectKey=test  -Dsonar.projectName='test' -Dsonar.host.url=http://3.107.55.196:9000   -Dsonar.login=${SONAR_TOKEN}'''
                    
                }
            }
        }  */
		
		
		
		stage('Code Analysis-2') {
            steps {
                script {
                    // Run SonarQube scan
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh "${MAVEN_HOME}/bin/mvn sonar:sonar  -Dsonar.java.binaries=. -Dsonar.projectKey=test  -Dsonar.projectName='test' -Dsonar.host.url=http://3.107.55.196:9000   -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        } 
		
		
	
		
		
/*		 stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'sonarqube-scanner';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'sonarqube-token', installationName: 'sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=test -Dsonar.projectName='test' -Dsonar.java.binaries=. "
              }
            }
        }  */  
		
		
		  stage('Quality Gate') {
            steps {
                // Wait for SonarQube quality gate result
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        } 
		
		
	  
		
    }	
	}
 
		
		
