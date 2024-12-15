pipeline{
    agent {
        label 'worker'
    }
    stages{
        stage("Build and Push"){
            steps{
                sh '''
                     cd vote 
                     aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 266735804962.dkr.ecr.us-east-1.amazonaws.com
                     #docker build -t 266735804962.dkr.ecr.us-east-1.amazonaws.com/mynap:vote .
                     #docker push 266735804962.dkr.ecr.us-east-1.amazonaws.com/mynap:vote
                     docker build -t 266735804962.dkr.ecr.us-east-1.amazonaws.com/mynap:vote-v${BUILD_NUMBER} .
                     docker push 266735804962.dkr.ecr.us-east-1.amazonaws.com/mynap:vote-v${BUILD_NUMBER}
                '''
            }
        }
        stage("Deploy"){
            steps{
                sh '''
                kubectl  set image deployment vote vote=266735804962.dkr.ecr.us-east-1.amazonaws.com/mynap:vote-v${BUILD_NUMBER}
                kubectl rollout restart deployment vote
                '''
            }
        }
    }
}
