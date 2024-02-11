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

        stage('Update Kustomize') {
            steps {
                script {
                    dir('k8s/overlays/production') {
                        sh "kustomize edit set image ${IMAGE_NAME}:${VERSION}"
	            	    sh "git config --global user.email rlozi1999@gmail.com"
                        sh "git config --global user.name LEJ"
                        sh "git add ."
                        sh "git commit -m 'Update image version to ${VERSION}'"
                        sh "git push origin main"
                    }
                }
            }
        }
    }
}

