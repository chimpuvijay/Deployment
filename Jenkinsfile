pipeline {
    agent any
    environment
     {
        registry = "madavi/springboot"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: ''
             
          }
        }
      stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
        }  
      stage('Image Build'){
           steps{
               script{
                     dockerImage = docker.build registry + ":$BUILD_NUMBER"
                 }
             }
         }
      stage('Deploy our image') {
          steps{
            script {
              docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
            }
        }
            }
        }
      stage('helm deploy') {
             
            steps {
               sh "helm repo add stable https://charts.helm.sh/stable"
               sh "helm repo update"
               sh "helm install springboot ."

}
}  
}
} 
