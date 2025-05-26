@Library('jenkins-shared-lib-1') _

org.iti.Jpipeline.run([
    script: this,
    docker_user: env.DOCKER_USR,
    docker_pass: env.DOCKER_PSW,
    default_image: "hk2802/java-mvn-sample",
    version: params.VERSION,
    skipTest: params.TEST
])
