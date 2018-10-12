node {
    withCredentials([
      [$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS'],
    ]) {
        stage('Login') {
           
            sh """
                docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}
            """
        }
        stage('Preparation') {
          git 'https://github.com/andrenarciso4/docker-project.git'
          
        }
        stage('Build') {
            
            app = docker.build("andrenarciso4/docker-jenkins","-f ./node/Dockerfile ./node")
        
        }
        stage('Push') {
            withDockerRegistry([credentialsId: 'docker-hub']) {
                app.push("${BUILD_NUMBER}")
                app.push("latest")
            }
        }
        stage('Deploy') {
            sh """
                helm upgrade --install docker-jenkins -f node/charts/javascript/values.yaml ./node/charts/javascript \
				--set image.repository=andrenarciso4/docker-jenkins,image.tag=${BUILD_NUMBER},service.type=LoadBalancer
            """
        }
    }
}
