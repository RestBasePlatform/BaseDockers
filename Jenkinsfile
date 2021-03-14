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
        
        stage("Build docker images"){
            steps{
                parallel(
                    base_image: {
                    script{
                        dir(path: 'restbase/base_image') {
                            image_name = sh (
                                    script: 'echo ${PWD##*/}',
                                    returnStdout: true
                            ).trim()
                            dockerImageBaseImage = docker.build username+image_name
                        }
                    }
                }, 
                'test_image':{
                    script{
                        dir(path: 'restbase/tests_base_image') {
                            image_name = sh (
                                    script: 'echo ${PWD##*/}',
                                    returnStdout: true
                            ).trim()
                            dockerImageTest = docker.build username+image_name
                        }
                    }
                }
                )
            }
        }
    
        stage("Push"){
            steps{
                script{
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential ) {
                        if ((env.BRANCH_NAME == 'master') || (env.BRANCH_NAME == 'main')){
                            dockerImageBaseImage.push('latest')
                            dockerImageTest.push('latest')
                        }
                        else {
                            dockerImageBaseImage.push(env.BRANCH_NAME)
                            dockerImageTest.push(env.BRANCH_NAME)
                        }
                    }
                }
            }
        }

        stage("Clear"){
            steps{
                catchError (buildResult: 'SUCCESS', stageResult: 'SUCCESS'){
                    script{
                        sh 'docker rmi -f $(docker image ls -aq)'
                    }
                }
            }
        }
    }
}
