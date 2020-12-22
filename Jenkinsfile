pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/lingamnagani/nodeapp.git', branch:'master'
      }
    }
    stage('Deploy App') {
      steps {
      sshagent(['kserver']) {
         sh "scp-o StrictHostKeyChecking=no config.yaml ubuntu@13.233.66.168:/home/ubuntu/"
         script {
          try {
                sh "ssh ubuntu@13.233.66.168 kubectl apply -f ."
        } catch(error) {
             sh "ssh ubuntu@13.233.66.168 kubectl create -f ."
}
         }       
      }
   }
}
}
}
