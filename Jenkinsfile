pipeline {
agent any
options {
timestamps()
timeout(time: 15, unit: 'MINUTES')
}
triggers {
pollSCM('H/2 * * * *')
}
environment {
APP
NAME =
'usn-calculator'
_
}
stages {
stage('Build') {
steps {
sh 'python3 -m py_
compile calculator.py'
echo 'Syntax check passed'
}
}
stage('Test') {
steps {
sh 'python3 -m pytest -v --junitxml=test-results.xml'
}
}
stage('Package') {
steps {
sh 'mkdir -p dist'
sh 'tar -czf dist/${APP
NAME}-${BUILD
_
archiveArtifacts artifacts: 'dist/*
NUMBER}.tar.gz calculator.py'
_
.tar.gz'
, fingerprint: true
}
}
stage('Deploy to staging') {
steps {
input message: 'Deploy to staging?'
sh 'mkdir -p /tmp/usn-staging'
sh 'cp calculator.py /tmp/usn-staging/'
sh 'ls -l /tmp/usn-staging'
, ok: 'Deploy'
}
}
}
post {
always {
junit allowEmptyResults: true, testResults: 'test-results.xml'
}
success {
echo "Build ${env.BUILD
NUMBER} of ${env.APP
NAME} passed"
_
_
}
failure {
echo "Build ${env.BUILD
NUMBER} failed - read the Console Output"
_
}
}
}
