pipeline {
    agent any

    stages {
        stage("Git Clone") {
            steps {
                echo "passed"
                //git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/rahul-trs/Springboot-K8s-Jenkins.git'
            }
        }

        stage('Gradle Build') {
            steps {
                sh './gradlew build'
            }
        }

        stage("Docker build") {
            steps {
                sh 'docker version'
                sh 'docker build -t springboot-k8s-jenkins .'
                sh 'docker image list'
                sh 'docker tag springboot-k8s-jenkins rahul9198/springboot-k8s-jenkins:k8s-1'
            }
        }

        stage("Push Image to Docker Hub") {
            environment {
                DOCKER_HUB_USERNAME = credentials('DOCKER_HUB_USERNAME')
                DOCKER_HUB_PASSWORD = credentials('DOCKER_HUB_PASSWORD')
            }
            steps {
                sh 'docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD'
                sh 'docker push rahul9198/springboot-k8s-jenkins:k8s-1'
            }
        }

        stage("Kubernetes deployment") {
            steps {
                sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
            }
        }
    }
}
