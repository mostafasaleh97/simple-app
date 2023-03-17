pipeline {
    agent "jenkins-slave" 
    stages {
        stage('build') {
            steps {
                    withCredentials([usernamePassword(credentialsId: 'mostafa', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t mostafa001/project-app .
                            docker push mostafa001/project-app
        
                        """
                    }
                    
                
            }
        }
        // stage('deploy') {
        //     steps {
            
        //             withCredentials([file(credentialsId: 'kubernetes_kubeconfig', variable: 'config')]) {
        //             sh """
        //                 export BUILD_NUMBER=\$(cat ../bakehouse-build-number.txt)
        //                 mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
        //                 cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
        //                 rm -f Deployment/deploy.yaml.tmp
        //                 kubectl apply -f Deployment --kubeconfig=${config}
        //             """
        //         }
            
                
        //     }
        // }
    }
}
