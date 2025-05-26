@Library('jenkins-shared-lib-1') _

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
                        def docker = new org.iti.Docker()
                        docker.build(env.DEFAULT_IMAGE, params.VERSION)
                }
            }
        }

        stage("push docker image"){
            steps {
                script {
                        def docker = new org.iti.Docker()
                        docker.login(env.DOCKER_USR, env.DOCKER_PSW)
                        docker.push(env.DEFAULT_IMAGE, params.VERSION)
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