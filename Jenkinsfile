pipeline {
    agent any
    environment {
       GIT_REPOSITORY_URL = 'https://github.com/cdac3642/docker_jenkins_demo.git'
       DOCKER_IMAGE_NAME = 'cdac3642/docker_jenkins_demo'
       IMAGE_TAG = '1.0'
}
   stages { 
        stage('Clone Repository') {
    steps { 
       script {
           try {
                git branch: 'main', url: GIT_REPOSITORY_URL
          } catch (Exception e) {
               echo "Failed to clone repository : ${e.message}"
               error "Failed to clone Repository "
            }
         }
       }
     }
   stage('Build Docker Image') {
          steps {

               script {

                    try {
                          docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                  } catch (Exception e) {
               echo "Failed to clone repository : ${e.message}"
               error "Failed to clone Repository "
                   }
                 }
             }
          }
     stage('Push to DockerHub') {
         steps {
             script {
                   try {
                        withCredentials([usernamePassword(credentialsId: 'docker-hub-raj' , usernameVariable: 'DOCKER_USERNAME' , passwordVariable: 'DOCKER_PASSWORD')]) {
                                 // Explicit login before push 
                                  sh """
                                  echo "$DOCKER_PASSWORD" | docker login -u
             "$DOCKER_USERNAME"  --password-stdin 
                                docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                              """
                          }
                    } catch (Exception e) {
               echo "Failed to push docker image to registry : ${e.message}"
               error "Failed to  push docker image "
               }   
            }
          }
       }
     }
   }


                   
