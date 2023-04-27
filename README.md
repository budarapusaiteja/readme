# readme

Pipeline Script:
pipeline {
 agent any
 tools {
 // Install the Maven version configured as "M3" and add it to the path.
 maven "maven_home"
 }
 stages {
 stage('Build') {
 steps {
 // Get some code from a GitHub repository
 git 'https://github.com/budarapusaiteja/Teja-Employee.git'
 // Run Maven on a Unix agent.
 sh "mvn clean package"
 }
 post {
 // If Maven was able to run the tests, even if some of the test
 // failed, record the test results and archive the jar file.
 success {
 archiveArtifacts 'target/*.jar'
 }
 } 
 }
 stage('build docker image'){
 steps{
 script{
 sh 'sudo docker build -t teja0102/springboot-crud-k8s .'
 }
 }
 }
 stage('push docker image to docker hub'){
 steps{
 script{
 sh 'sudo docker login -u teja0102 -p Teja0102@'
 sh 'sudo docker push teja0102/springboot-crud-k8s:latest'
 }
 }
 }
 }
}
