pipeline {
    agent any
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t  gsaini05/nodeapp:${Docker_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u lingamnagani -p ${dockerHubPwd}"
                    sh "docker push lingamnagani/dockertestfile:${Docker_TAG}"
                }
            }
        }
        stage('Deploy App') {
         steps {
              sh "chmod +x changeTag.sh"
              sh "./changeTag.sh ${Docker_TAG}"
              sshagent(['kserver']) {
                  sh "scp-o StrictHostKeyChecking=no services.yml node-app-pod.yml ubuntu@13.233.66.168:/home/ubuntu/"
                script{
                  try{
                    sh "ssh ubuntu@13.233.66.168 kubectl apply -f ."
                  } catch(error){
                    sh "ssh ubuntu@13.233.66.168 kubectl create -f ."
                    
}
					                }
            }
        }
    }
}
}
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
