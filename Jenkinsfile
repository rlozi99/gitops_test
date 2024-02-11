pipeline {
    agent any

    environment {
        IMAGE_NAME = 'aksregigi.azurecr.io/my-simple-website'
        VERSION = "v${new Date().format('yyyyMMddHHmmss')}" // 예시 태그 버전
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${VERSION} ."
                }
            }
        }

        stage('Logingit  to ACR') {
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

        stage('Update Kustomize') {
            steps {
                script {
                    dir('k8s/overlays/production') {
                        sh "git config --global user.email 'rlozi1999@gmail.com'"
                        sh "git config --global user.name 'LEJ'"
                        sh "git branch"
                        sh "git fetch --all"
                        sh "git reset --hard origin/main"
                        sh "git branch -M main"
                        sh "git remote add origin https://github.com/rlozi99/gitops_test.git"
                        sh "kustomize edit set image ${IMAGE_NAME}=${IMAGE_NAME}:${VERSION}"
                        sh "git add ."
                        sh "git commit -m 'Update image version to ${VERSION}'"
                        sh "git push -u origin main"
                    }
                }
            }
        }
    }
}