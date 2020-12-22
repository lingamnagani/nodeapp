pipeline {
    agent any
    environment{
          Docker_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t kammana/nodeapp:${Docker_TAG} "
            }
        }
        stage('Deploy App') {
         steps{
              sh "chmod +x changeTag.sh"
              sh "./changeTag.sh ${Docker_TAG}"
              sshagent(['sampledemo']) {
                  sh "scp -o  StrictHostKeyChecking=no /var/lib/jenkins/workspace/kube1/services.yml ubuntu@13.233.66.168:/home/ubuntu/"
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
