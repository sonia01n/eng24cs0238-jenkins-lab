
pipeline {
    agent any

    environment {
        APP_NAME = 'eng24cs0238-calculator'
    }

    triggers {
        pollSCM('H/2 * * * *')
    }

    options {
        timestamps()
        timeout(time: 15, unit: 'MINUTES')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building calculator project'
                sh 'ls -l'
                sh 'python3 --version'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Python tests'
                sh 'python3 -m pytest -v --junitxml=test-results.xml'
            }
            post {
                always {
                    junit 'test-results.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application'
                sh 'tar -czf calculator.tar.gz calculator.py test_calculator.py'
                archiveArtifacts artifacts: 'calculator.tar.gz', fingerprint: true
            }
        }

        stage('Deploy to Staging') {
            steps {
                input message: 'Approve deployment to staging?', ok: 'Deploy'
                echo 'Application deployed to staging!'
            }
        }
    }
}
