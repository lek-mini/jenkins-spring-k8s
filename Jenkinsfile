pipeline {
    agent {
        kubernetes {
          yamlFile 'k8s/KubernetesPod.yaml'
        }
    }
    stages {
        stage('Build') {
            steps {
                container('maven') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }
        stage('Test') {
            steps {
                container('maven') {
                    sh 'mvn test'
                }
            }
        }
        stage('Build Image') {
            steps {
                container('docker') {
                     sh 'docker build -t localhost:32000/hello-jenkins-maven .'
                }
            }
        }
        stage('Push Image') {
            steps {
                container('docker') {
                     sh 'docker push localhost:32000/hello-jenkins-maven'
                }
            }
        }
        stage('Deployment') {
            steps {
                container('kubectl') {
                     sh 'kubectl apply -f k8s/Deployment.yaml -n default'
                }
            }
        }
    }
}
