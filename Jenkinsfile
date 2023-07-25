// node {
// 	stage('Build') {
// 		echo "Build"
// 	}
// 	stage('Test') {
// 		echo "Test"
// 	}
// }


pipeline{
	// agent{docker {image 'maven:3.6.3'}}
	agent any
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages{
		stage('Checkout'){
			steps{
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
			}
		}
		stage('Compile'){
			steps{
				sh "mvn clean compile"
			}
		}

		stage('Test'){
			steps{
				sh " mvn test"
			}
		}
		stage('Integration Test'){
			steps{
				// echo "Integration Test"
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}

		stage('Package'){
			steps{
				// echo "Integration Test"
				sh "mvn package -Dskip"
			}
		}

			stage('Build Docker Image'){
		steps{
			// "docker build -t shresthabibek/currency-exchange-devops:$env.BUILD_TAG"
			script{
				dockerImage = docker.build("shresthabibek/currency-exchange-devops:${env.BUILD_TAG}")
			}
		}
	}
	stage('Push Docker Image'){
		steps{
			script{
				docker.withRegistry('','dockerhub'){
					dockerImage.push();
					dockerImage.push('latest');
				}
				
			}
		}
	}
	}


	
	post{
		always{
			echo 'Im awesome. I run always'
		}
		success{
			echo 'I run when you are successful'
		}
		failure{
			echo 'I run when you fail'
		}
	}
}