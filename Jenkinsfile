@Library('jenkins-shared-lib-1') _
import org.iti.Docker

pipeline{
    agent {
        label 'agent-1'
    }

    tools {
        jdk "java-8"
        maven "maven-3.8"
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
            steps {
                script {
                        def docker = new Docker(
                        image: env.DEFAULT_IMAGE,
                        tag: params.VERSION
                    )
                    docker.steps = this
                    docker.build()
                }
            }
        }

        stage("push docker image"){
            steps {
                script {
                        def docker = new Docker(
                        docker_user: env.DOCKER_USR,
                        docker_pass: env.DOCKER_PSW,
                        image: env.DEFAULT_IMAGE,
                        tag: params.VERSION
                    )
                    docker.steps = this
                    docker.login()
                    docker.push()
                }
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