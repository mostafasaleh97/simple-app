pipeline {
    agent { label 'jenkins-slave' }
    stages {
            stage('sonarqube'){
                steps {
                    script {
                        def pipelineConfig=[
                            sonarQubeServer: 'sonarqube',
                        ]
                        def repositoryUrl = scm.userRemoteConfigs[0].getUrl()
                        def GIT_REPO_NAME = scm.userRemoteConfigs[0].getUrl().tokenize('/').last().split("\\.")[0]
                        def scannerHome = tool 'sonar_tool'
                        def SONAR_BRANCH_NAME = env.BRANCH_NAME
                        withSonarQubeEnv("sonarqube-iti") {
                            sh "sed -i s#{{repo_name}}#${GIT_REPO_NAME}# sonar-project.properties"
                            sh "sed -i s#{{branch_name}}#${SONAR_BRANCH_NAME}# sonar-project.properties"
                            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectVersion=${SONAR_BRANCH_NAME} -Dsonar.buildString=Jenkins-${SONAR_BRANCH_NAME}-BLD${env.BUILD_NUMBER}"
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
