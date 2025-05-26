@Library('jenkins-shared-lib-1') _

pipeline{
    agent {
        label 'agent-1'
    }

    tools {
        jdk "java-8"
    }

    environment{
        DOCKER = credentials('docker-hub-login')
        DEFAULT_IMAGE = "hk2802/java-mvn-sample"
    }

    parameters {
        string defaultValue: '${BUILD_NUMBER}', description: 'Enter the version of the docker image', name: 'VERSION'
        choice choices: ['true', 'false'], description: 'Skip test', name: 'TEST'
    }

    stages{
        stage("VM info"){
            steps{
                script{
                    echo "Agent IP: ${vmIp()}"
                }
            }
        }

        stage("Build java app"){
            steps{
                sayHello 'ITI'
                mvnBuild(params.TEST)
            }
        }

        stage("build docker image"){
            steps{
                dockerBuild image: DEFAULT_IMAGE,
                            tag:   params.VERSION
            }
        }

        stage("push docker image"){
            steps{
                dockerLogin docker_user: env.DOCKER_USR,
                            docker_pass: env.DOCKER_PSW

                dockerPush  image: DEFAULT_IMAGE,
                            tag:   params.VERSION
            }
        }
    }

    post {
        always {
            echo 'Clean the Workspace'
            cleanWs()
        }
        failure {
            echo 'failed'
        }
    }
}