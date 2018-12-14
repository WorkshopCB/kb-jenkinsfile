pipeline {
    agent {
        kubernetes {
            label 'mypod'
            defaultContainer 'jnlp'
            yamlFile 'kb-podTemplate.yaml'
        }
    }
    triggers {
        eventTrigger simpleMatch('dcanadillas/spring-petclinic:latest')
    }
    stages {
        stage('Deploy') {
            steps {
                container('kubectl') {
                    echo 'Deploying k8 app'
                    //sh 'kubectl get pods -n staging'
                    sh 'kubectl get deployments -n staging'
                    sh 'kubectl -n staging delete service spring-petclinic'
                    sh 'kubectl -n staging delete deployment petclinic'
                    //sh 'kubectl apply -f test-deploy.yaml -n staging'
                    sh 'kubectl run spring-petclinic --image=dcanadillas/spring-petclinic:latest --port 8080 --namespace staging'
                    sh 'kubectl expose deployment spring-petclinic --type=LoadBalancer --port 8092 --target-port 8080 --namespace staging'
                    echo 'Spring Petclinic app has been deployed'

                }
            }
        }
        stage('Check') {
            steps {
                container('kubectl') {
                    echo 'Check Deployment and Service'
                    sh 'kubectl get deployments -n staging'
                    sh 'kubectl describe deployment petclinic -n staging'
                }
            }
        }
    }
}