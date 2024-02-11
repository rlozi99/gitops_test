pipeline {
    agent any // Jenkins에서 사용할 agent를 정의합니다.

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rlozi99/gitops_test.git', branch: 'main'
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
