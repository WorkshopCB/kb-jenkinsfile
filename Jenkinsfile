pipeline {
    agent {
        kubernetes {
            label 'mypod'
            defaultContainer 'jnlp'
            yamlFile 'kb-podTemplate.yaml'
        }
    }
    stages {
        stage('Deploy') {
            steps {
                container('kubectl') {
                    echo 'Deploying k8 app'
                    sh 'kubectl apply -f test-deploy.yaml -n staging'
                }
            }
        }
        stage('Check') {
            steps {
                container('kubectl') {
                    echo 'Check Deployment and Service'
                    sh 'kubectl get svc -n staging'
                    sh 'kubectl get deployments -n stating'
                }
            }
        }
    }
}