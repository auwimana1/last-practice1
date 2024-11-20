pipeline{
    agent any

    parameters{
        string (name:'KOSSI' , defaultValue: '1.0.1')
    }

    environment{
        ECR_REPO_URL='557195342730.dkr.ecr.us-east-1.amazonaws.com' //THIS VARIABLE IS FOR MY ECR REPOSITORY
        APP_NAME='wedidit' // THIS VARIABLE IS USED TO AS A NAME OF MY IMAGE TO MY CURRENT DOCKER (EC2 INSTANCE)
        ECR_APP_NAME='wedidit'// THIS VARIABLE IS USED TO GIVE THE NEW NAME IMAGE IN ECR
        CRED_ID='ecr:us-east-1:Adelina_IAM' //THIS VARIABLE IS FOR MY AWS CREDENTIAL PUT IN JENKINS
        ECS_CLUSTER_NAME='Dev-ecs-cluster' //MY ECS CLUSTER NAME
        ECS_SVC_NAME='dev-cluster' //MY ECS SERVICE NAME
        MY_URL='https://${ECR_REPO_URL}/'
    }


    stages{

        stage('Scan files'){
            steps{
                sh "trivy fs --format table -o adedidthat.html ."
            }
        }


        stage('Build image'){
            steps{
            sh "docker build -t ${APP_NAME}:${params.KOSSI} ."
            sh "docker tag ${APP_NAME}:${params.KOSSI} ${ECR_REPO_URL}/${ECR_APP_NAME}:${params.KOSSI}"
            sh "docker tag ${APP_NAME}:${params.KOSSI} ${ECR_REPO_URL}/${ECR_APP_NAME}:latest"

            }
        }


        // stage('Scan image'){
        //     steps{
        //         sh "trivy image --format table -o docker_image_adedidthat.html ${ECR_REPO_URL}/${ECR_APP_NAME}:latest"
        //     }
        // }


        stage('Push ECR'){
            steps{
                script{
                // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: CRED_ID, url: 'https://${ECR_REPO_URL}/') {
                    sh "docker push ${ECR_REPO_URL}/${ECR_APP_NAME}:${params.KOSSI}"
                    sh "docker push ${ECR_REPO_URL}/${ECR_APP_NAME}:latest"
}
                }
            }
        }


        stage('Update images to ecs'){
            steps{
                 sh "aws ecs update-service --cluster ${ECS_CLUSTER_NAME} --service  ${ECS_SVC_NAME} --force-new-deployment"
            }
              
        }
    }
}