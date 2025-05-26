@Library('jenkins-shared-lib-1') _

Jpipeline([
    script: this,
    docker_user: env.DOCKER_USR,
    docker_pass: env.DOCKER_PSW,
    default_image: "hk2802/java-mvn-sample",
    version: params.VERSION,
    skipTest: params.TEST
])
