pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/prabhu-dp/k8s-prj.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t newimage /var/lib/jenkins/workspace/k8s-prj'
                sh 'sudo docker tag newimage prabhudp/newimage:latest'
                sh 'sudo docker tag newimage prabhudp/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push prabhudp/newimage:latest'
                sh 'sudo docker image push prabhudp/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/k8s-prj/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}

