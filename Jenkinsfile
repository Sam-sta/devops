pipeline {
    agent {label 'linux'}
    parameters {
        string(name: 'image-name', defaultvalue: 'msmapp')

        string(name: 'target-image', defaultvalue: 'samsta/practice_jenkins')
    }
    tools {
        nodejs 'node16.8.0'
    }
    stages {
        stage('clone app repository') {
            steps {
                git branch: "main", url: "https://github.com/Sam-sta/AT-BC-LAT-01.git"
            }
        }

        stage('install dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('build app image') {
            steps {
                sh "docker build -t ${params.image-name}:$BUILD_NUMBER ."
            }
        }

        stage('docker-hub login') {
            environment {
                DOCKERHUB_CREDS = credentials('Dockerhub-login')
            }
            steps {
                sh "docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW"
            }
        }

        stage('tag image') {
            steps {
                sh "docker tag ${params.image-name}:$BUILD_NUMBER ${params.target-image}:${params.image-name}$BUILD_NUMBER"
            }
        }

        stage('push image') {
            steps {
                sh "docker push ${params.target-image}:${params.image-name}$BUILD_NUMBER"
            }
            post {
                always {
                    script {
                        sh "docker rmi -f ${params.target-image}:${params.image-name}$BUILD_NUMBER"
                        sh "docker rmi -f ${params.image-name}:$BUILD_NUMBER"
                        sh "docker logout"
                    }
                }
            }
        }
    }
}