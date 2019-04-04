pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }

		stage('SonarQube analysis') {
			
			steps {
				script {
					// requires SonarQube Scanner 2.8+
					scannerHome = tool 'SonarQube Scanner 2.8'
				}
				withSonarQubeEnv('SonarQube Scanner') {
					bat "C:/Users/selu0005/Programmering/sonar-scanner-3.3.0.1492-windows/bin/sonar-scanner"
				}
			}
	
		}
    }
}
