pipeline {
    agent {
        kubernetes {
         label 'jenkins-jenkins-agent'
        }
    }
    environment {
        DOCKERHUB_USERNAME = "hazemhashem100"
        APP_NAME = "nodejs-app"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${BUILD_NUMBER}"

    }
    stages {


         stage('build') {
             steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                     sh 'docker login -u ${USERNAME} --password ${PASSWORD} '
                      sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                 }
             }
         }
         stage('push') {
             steps {
                 script{
        
                         sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                 }
          
             } 
         }

        stage('Github') {
            steps {
                 sh "git checkout rds_redis"
               sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' ${WORKSPACE}/deployment/DeploymentApp.yml "

                script{
                    sh """
                    git config --global user.name "hazem"
                    git config --global user.email "hazemhaashem@gmail.com"
                    git add .
                    git commit -m "update the appdeploy file"
                        """
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh "git push https://$USERNAME:$PASSWORD@github.com/hazemhashem/testdevops.git rds_redis "
                     }        
                                                


                    
                } 

              
            }
        }

    }
}




