pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Poojamarne-123/hrms-eks-cicd-v2.git'
            }
        }
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh '''
                    npm install
                    npm run build
                    '''
                }
            }
        }
        stage('Build Backend Image') {
            steps {
                sh 'docker build -t hrms-backend ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t hrms-frontend ./frontend'
            }
        }
	stage('Login to ECR') {
	    steps {
  	        sh '''
 	        aws ecr get-login-password --region us-east-1 \
 	        | docker login \
	        --username AWS \
	        --password-stdin 930664225702.dkr.ecr.us-east-1.amazonaws.com
	        '''
 	   }
	}
    }
}
