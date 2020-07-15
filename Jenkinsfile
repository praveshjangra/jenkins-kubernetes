pipeline {
   agent any
   stages{
   stage("Git checkout"){
   steps{
   git credentialsId: 'jenkins', url: 'git@github.com:praveshjangra/jenkins-kubernetes.git'
   }
   }
   }
   }
