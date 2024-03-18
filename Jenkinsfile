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
   

       stage('EKS-Deploy') {
        steps{   
            script {
		    withKubeConfig(
			    credentialsId: 'EKS',
			    clusterName: 'java-python-cluster',
			    contextName: 'arn:aws:eks:ap-south-1:637423572777:cluster/java-python-cluster',
			    serverUrl: 'https://12FB5133E5192CF9F1F405C968089526.yl4.ap-south-1.eks.amazonaws.com'
		    ) {
			    sh ('kubectl apply -f eks-deploy-k8s.yaml')
		    }
                }
            }
        }
       }
    }
