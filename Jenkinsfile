pipeline {
    agent any
    stages {
    // Building Docker images
        stage('ECR push') {
            steps {     
		    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 637423572777.dkr.ecr.ap-south-1.amazonaws.com'
	        sh 'docker build -t eks-jenkins-repo:v${BUILD_NUMBER} -t  eks-jenkins-repo:latest .'
	        sh 'docker tag eks-jenkins-repo:v${BUILD_NUMBER} 637423572777.dkr.ecr.ap-south-1.amazonaws.com/eks-jenkins-repo:v${BUILD_NUMBER}'
		    sh 'docker tag eks-jenkins-repo:v${BUILD_NUMBER} 637423572777.dkr.ecr.ap-south-1.amazonaws.com/eks-jenkins-repo:latest'
	        sh 'docker push 637423572777.dkr.ecr.ap-south-1.amazonaws.com/eks-jenkins-repo:v${BUILD_NUMBER}'
		    sh 'docker push 637423572777.dkr.ecr.ap-south-1.amazonaws.com/eks-jenkins-repo:latest'
            }
        }
   

       stage('EKS Deploy') {
        steps{   
            script {
                withKubeConfig([credentialsId: 'EKS', serverUrl: '']) {
                sh ('kubectl apply -f  eks-deploy-k8s.yaml')
                }
            }
        }
       }
    }
}
