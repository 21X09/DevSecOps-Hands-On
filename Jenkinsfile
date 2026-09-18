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
		stage('Docker Build'){
			steps{
				sh 'docker build -t devsecops-hands-on:${BUILD_NUMBER} .'
			}
		}
		stage('SAST-semgrep'){
			steps{
				sh '''
					docker run --rm -v "$WORKSPACE:/src" returntocorp/semgrep \
					semgrep scan --config=auto --error /src
					'''
			}
		}
		stage('SCA-Trivy'){
			steps{
				sh '''
					docker run --rm -v "$WORKSPACE:/src" aquasec/trivy \
					fs --scanners vuln --exit-code 1 --severity HIGH,CRITICAL /src
					'''
			}
		}
	}
	post{
		always{
			junit 'target/surefire-reports/*.xml'
		}
	}
}