pipeline {
	agent any
	tools {
		maven "Maven3"
	}
	stages{
		stage('Checkout'){
			steps{
				checkout scm
			}
		}
		stage('Build'){
			steps{
				sh 'mvn -B compile'
			}
		}
		stage('Test'){
			steps{
				sh 'mvn -B test'
			}
		}
	}
	post{
		always{
			junit 'target/surefire-reports/*.xml'
		}
	}
}