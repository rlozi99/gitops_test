pipeline {
    agent any

    environment {
        // 환경변수 설정
        IMAGE_NAME = 'aksregigi.azurecr.io/my-simple-website'
        VERSION_TAG = "v${new Date().format('yyyyMMddHHmmss')}"
        REGISTRY_CREDENTIALS = 'jenkins-acr-access' // Jenkins에 저장된 ACR의 인증 정보 ID
        GIT_CREDENTIALS = 'github-access-token' // Jenkins에 저장된 GitHub의 인증 정보 ID
        GIT_REPOSITORY = 'rlozi99/gitops_test' // GitHub 저장소 경로
    }

    stages {
        // Git 저장소로부터 소스 코드를 체크아웃
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        // Docker 이미지 빌드
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${VERSION_TAG} ."
                }
            }
        }

        // ACR에 로그인
        stage('Login to ACR') {
            steps {
                script {
                    sh "docker login ${IMAGE_NAME} -u <username> -p <password>"
                }
            }
        }

        // Docker 이미지를 ACR에 푸시
        stage('Push to ACR') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:${VERSION_TAG}"
                }
            }
        }

        // Kustomize 설정 업데이트
        stage('Update Kustomize') {
            steps {
                script {
                    dir('k8s/overlays/production') {
                        sh "kustomize edit set image ${IMAGE_NAME}=${IMAGE_NAME}:${VERSION_TAG}"
                        sh "git add ."
                        sh "git commit -m 'Update image version to ${VERSION_TAG}'"
                    }
                }
            }
        }

        // Git에 변경 사항 푸시
        stage('Git Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: GIT_CREDENTIALS, passwordVariable: 'GITHUB_TOKEN')]) {
                        sh "git push https://${GITHUB_TOKEN}@github.com/${GIT_REPOSITORY}.git main"
                    }
                }
            }
        }

        // 이후 스테이지들...
    }

    // 오류 발생 시 포스트 처리
    post {
        always {
            // 워크스페이스 정리 등의 포스트 작업
        }
    }
}