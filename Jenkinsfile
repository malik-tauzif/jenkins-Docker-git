pipeline {
	agent any
	environment {
		dockerHome = tool 'mydocker'
		MavenHome = tool 'mymaven'
		PATH = "$dockerHome/bin:$MavenHome/bin:$PATH"
	}
	stages {	
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "$PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
			}
	    }
		
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		
		stage('Test') {
			steps {
				sh "mvn test"
			}
	    }
		
		stage('Integration testing') {
			steps {
		     	sh "mvn failsafe:integration-test failsafe:verify"
			}	 
	    }
		
		stage('Package') {
			steps {
		     	sh "mvn package -DskipTests"
			}	 
	    }
		
		stage('Build Docker Image') {
			steps {
				//"docker build -t in28min/currency-exchange-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("maliktauseef/hello-world:${env.BUILD_TAG}")
				}

			}
		}
		
		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry('', 'dockerhub') {
						dockerImage.push();
						dockerImage.push('latest');
					}	
				}
				
			}
		}
	}	
}