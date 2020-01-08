pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
      
     ECR_URI = "842970055596.dkr.ecr.ap-south-1.amazonaws.com"
     
     SERVICE_NAME = "fleetman-position-tracker"
     REPOSITORY_TAG="${ECR_URI}/${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Build') {
         steps {
            sh '''mvn clean package'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
            docker {
               image '${REPOSITORY_TAG}'
               registryUrl '${ECR_URI}'
               registryCredentialsId 'ecr:us-east-1:ECRLOGIN'
            }
               
               
            sh 'docker push ${REPOSITORY_TAG}'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
