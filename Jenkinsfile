pipeline{
    agent any
    stages{
    stage('build') {
      steps {
        script {
          def dockerHome = tool 'docker'
          env.PATH = "${dockerHome}/bin:${env.PATH}"
          // The Docker resource value is docker.repo1.uhc.com
           String containerId = sh(script: "sudo docker build -f Dockerfile ./ | tail -1", returnStdout: true).split(' ')[2].trim()
            echo "Container Id: ${containerId}"
            String dockerPushResource = "samba1236/spring-boot:kubernetes"

            // Make a tag and push
            sh "sudo docker tag ${containerId} ${dockerPushResource}"
            // Login to the Artifactory Docker registry
             sh "sudo docker login -u samba1236 -p Samba@1236 docker.io"
             sh "sudo docker push ${dockerPushResource}"
            }
        }
      }
      stage('deploy') {
            steps {
              script {
                    kubernetesDeploy(
                        configs: 'springboot.yaml',
                        kubeconfigId: 'k8s',
                        enableConfigSubstitution: true
                        )
                  }
              }
            }

  }
}
