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
                  sh "scp -o  StrictHostKeyChecking=no services.yml ubuntu@13.233.66.168:/home/ubuntu/"
                  script{
                      try{
                          sh "ssh ubuntu@13.233.66.168 kubectl apply -f services.yml"
                      } catch(error){
                           sh "ssh ubuntu@13.233.66.168 kubectl create -f services.yml"
                   
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
