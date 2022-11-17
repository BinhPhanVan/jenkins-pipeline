pipeline {

  agent none

  environment {
    DOCKER_IMAGE_BACKEND = "binhphanvan/goshipdemo"
  }

  stages {
    stage("build image backend") {
      agent { node {label 'master'}}
      environment {
        DOCKER_TAG="${GIT_BRANCH.tokenize('/').pop()}-${GIT_COMMIT.substring(0,7)}"
      }
      steps {
        sh "docker build -t ${DOCKER_IMAGE_BACKEND}:${DOCKER_TAG} . "
        sh "docker tag ${DOCKER_IMAGE_BACKEND}:${DOCKER_TAG} ${DOCKER_IMAGE_BACKEND}:latest"
        sh "docker image ls | grep ${DOCKER_IMAGE_BACKEND}"
        sh "docker run -p 7009:8000 ${DOCKER_IMAGE_BACKEND}"
//         withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
//             sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
//             sh "docker push ${DOCKER_IMAGE_BACKEND}:${DOCKER_TAG}"
//             sh "docker push ${DOCKER_IMAGE_BACKEND}:latest"
//         }

        //clean to save disk
        sh "docker image rm ${DOCKER_IMAGE_BACKEND}:${DOCKER_TAG}"
        sh "docker image rm ${DOCKER_IMAGE_BACKEND}:latest"
      }
    }
  }

  post {
    success {
      echo "SUCCESSFUL"
    }
    failure {
      echo "FAILED"
    }
  }
}
