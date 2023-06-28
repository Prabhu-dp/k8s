pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/Iam-mithran/kuber.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t newimage /var/lib/jenkins/workspace/kuber'
                sh 'sudo docker tag newimage iammithran/newimage:latest'
                sh 'sudo docker tag newimage iammithran/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push iammithran/newimage:latest'
                sh 'sudo docker image push iammithran/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/kuber/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
