pipeline {
  agent any 
  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        //git branch: 'main', url: 'https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git'
      }
    }
    stage('Build and Push Docker Image') {
      environment {
        DOCKER_IMAGE = "rahul9198/springboot-k8s-jenkins:${BUILD_NUMBER}"
        // DOCKERFILE_LOCATION = "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                dockerImage.push()
            }
        }
      }
    }
    stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "Springboot-K8s-Jenkins"
            GIT_USER_NAME = "rahul-trs"
        }
        steps {
            withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    /* git checkout master
                    git config user.email "rahul.xyz@gmail.com"
                    git config user.name "Rahul"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" k8s-spring-boot-deployment.yml
                    git add k8s-spring-boot-deployment.yml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git pull origin master
                    \* git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:master
                    echo 'echo passed'
                '''
            }
        }
    }
    stage("Kubernetes deployment") {
            steps {
                sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
            }
        }    

    
  }
}
