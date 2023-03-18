pipeline {
    agent { label 'jenkins-slave' }
    stages {
            stage('sonarqube'){
                steps {
                    script {
                        def pipelineConfig=[
                            sonarQubeServer: 'sonarqube',
                        ]
                        def scannerHome = tool 'sonar_tool'
                        withSonarQubeEnv("sonarqube-iti") {
                            sh "${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=simple-app \
                                -Dsonar.sources=src "
                        }
                        //TODO: enable step (requires webhook on Sonarqube server)
                        //timeout(time: 10, unit: 'MINUTES') {
                            // waitForQualityGate abortPipeline: true
                        //} 
                    }
                }
        }
        stage('build') {
            steps {
                    withCredentials([usernamePassword(credentialsId: 'mostafa', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh """
                            docker login -u $USERNAME -p $PASSWORD
                            docker build -t mostafa001/simple-app .
                            docker push mostafa001/simple-app
        
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
