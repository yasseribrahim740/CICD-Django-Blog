pipeline {
    agent any
 
       environment {
        KUBECONFIG = credentials('yasser')
    }
    stages {
    
          stage('build the image') {
            steps {
               sh """ 
                cd ./blog/
                docker build -t yasser74/django_web_blog:${BUILD_NUMBER} . """
            }
        }
          stage('pushing container to rpeo') {
            steps {
                
              withCredentials( [ usernamePassword(credentialsId: 'docker-hub' , passwordVariable: 'PASS' , usernameVariable: 'USER' )] ) {
                         sh " docker login -u  $USER -p $PASS "
                         sh " docker push yasser74/django_web_blog:${BUILD_NUMBER} "
                        }
            
                                         
            }

        }
        stage('deploy to k8s') {
            steps {   
            withCredentials([file(credentialsId: 'yasser', variable: 'KUBECONFIG_PATH')]) {
            sh '''
            kubectl version
            kubectl apply -f ./k8s/ --kubeconfig=${KUBECONFIG_PATH}
            kubectl get po -A
             '''
        }
            sh' kubectl get po -A'
            
                                         
            }

        }
    }
}
