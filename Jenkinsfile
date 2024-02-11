pipeline {
    agent any // Jenkins에서 사용할 agent를 정의합니다.

    environment {
        // 환경 변수 설정
        ACR_NAME = 'aksregigi' // 실제 ACR 레지스트리 이름으로 대체
        IMAGE_NAME = 'aksregigi.azurecr.io/my-simple-website' // 실제 이미지 이름으로 대체
        IMAGE_TAG = 'latest' // 실제 이미지 태그로 대체
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rlozi99/gitops_test.git', branch: 'main'
            }
        }

        stage('Login to ACR') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'jenkins-acr-access', usernameVariable: 'ACR_USER', passwordVariable: 'ACR_PASSWORD')]) {
                        sh 'docker login aksregigi.azurecr.io --username $ACR_USER --password $ACR_PASSWORD'
                    }
                }
            }
        }

        stage('Push to ACR') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:${VERSION}"
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'echo "Build step 실행중..."'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'echo "Test step 실행중..."'
                // 테스트 명령어를 여기에 추가하세요.
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying..'
                sh 'echo "Deploy step 실행중..."'
                // 배포 명령어를 여기에 추가하세요.
            }
        }
    }
}
