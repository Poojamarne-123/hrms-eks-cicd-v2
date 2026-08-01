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
                sh '''
                docker build -t hrms-backend ./backend
                '''
            }
        }


        stage('Build Frontend Image') {
            steps {
                sh '''
                docker build -t hrms-frontend ./frontend
                '''
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


        stage('Push Images') {
            steps {
                sh '''
                docker tag hrms-backend \
                930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-backend:latest


                docker push \
                930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-backend:latest



                docker tag hrms-frontend \
                930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-frontend:latest


                docker push \
                930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-frontend:latest
                '''
            }
        }


        stage('Deploy to EKS') {
            steps {
                sh '''
                echo "Updating backend image"

                kubectl set image deployment/hrms-backend \
                hrms-backend=930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-backend:latest \
                -n hrms



                echo "Updating frontend image"

                kubectl set image deployment/hrms-frontend \
                hrms-frontend=930664225702.dkr.ecr.us-east-1.amazonaws.com/hrms-frontend:latest \
                -n hrms



                echo "Waiting for backend rollout"

                kubectl rollout status deployment/hrms-backend \
                -n hrms



                echo "Waiting for frontend rollout"

                kubectl rollout status deployment/hrms-frontend \
                -n hrms
                '''
            }
        }

    }
}
