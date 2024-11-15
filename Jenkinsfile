pipeline{
    agent any

    parameters{
        string (name:'KOSSI' , defaultValue: '1.0.1')
    }

    stages{
        stage('build image to ecr'){
            steps{
                script{
                   // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'ecr:us-east-1:last-practice12', url: 'https://557195342730.dkr.ecr.us-east-1.amazonaws.com/') {
                    sh "docker build -t wedidit:${params.KOSSI} ."
                    sh "docker tag wedidit:${params.KOSSI} 557195342730.dkr.ecr.us-east-1.amazonaws.com/wedidit:${params.KOSSI}"
                    sh "docker tag wedidit:${params.KOSSI} 557195342730.dkr.ecr.us-east-1.amazonaws.com/wedidit:latest"
                    sh "docker push 557195342730.dkr.ecr.us-east-1.amazonaws.com/wedidit:latest"
}
                }
            }
        }
    }
}