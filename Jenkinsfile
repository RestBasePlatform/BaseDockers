#! groovy

pipeline {
    environment {
        username = "restbase/"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    agent any

    stages {
        stage("Checkout"){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '**']], extensions: [], userRemoteConfigs: [[credentialsId: 'restbaseapi_git', url: 'https://github.com/RestBaseApi/BaseDockers']]])
            }
        }

        stage("Build"){
            steps{
                script{

                    dir(path: 'restbase/base_image') {
                        image_name = sh (
                                script: 'echo ${PWD##*/}',
                                returnStdout: true
                        ).trim()
                        dockerImage = docker.build username+image_name
                    }
                }
            }
        }
        stage("Push"){
            steps{
                script{
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential ) {
//                        dockerImage.push('latest')
                        if ((env.BRANCH_NAME == 'master') || (env.BRANCH_NAME == 'main')){
                            dockerImage.push('latest')
                        }
                        else {
                            dockerImage.push(env.BRANCH_NAME)
                        }
                    }
                }
            }
        }

        stage("Clear"){
            steps{
                script{
                    sh 'docker rmi -f $(docker image ls -aq)'
                }
            }
        }
    }
}
