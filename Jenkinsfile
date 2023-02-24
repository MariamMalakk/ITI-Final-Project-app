pipeline {
    agent { label 'iti-slave' }

    stages {
     
        stage('build') {
            steps {
                script {
                       withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                           sh """
                                docker login -u $USERNAME -p $PASSWORD
                                docker build -t mariammalak/iti-final-app:${BUILD_NUMBER} ./app/
                                docker push mariammalak/iti-final-app:${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../app-build-number.txt
                           """
                       }
                    }
                }
            }
       
        
        stage('deploy') {
            steps {
                script {
     
                          sh """
                              export BUILD_NUMBER=\$(cat ../app-build-number.txt)
                              mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                              cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                              rm -f Deployment/deploy.yaml.tmp
                              kubectl create ns my-app
                              kubectl apply -f Deployment -n my-app
                            """
                        }
                    }
                }
            }
         }

