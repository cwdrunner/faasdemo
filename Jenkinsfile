def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label,
containers: [
    containerTemplate(privileged: true, name: 'dind', image: 'docker:dind', ttyEnabled: true, command: 'cat')
  ]) {
    node(label) {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/cwdrunner/faasdemo.git']]])

        stage("Docker info")  {
            container("dind") {
                sh "apk add --no-cache curl git && curl -sLS cli.openfaas.com | sh"
                sh "dockerd &"
                sh "docker run --rm --privileged multiarch/qemu-user-static --reset -p yes"
                sh "export DOCKER_CLI_EXPERIMENTAL=enabled"
                sh "docker info"
                sh "faas-cli template store pull java11"
                sh "faas-cli publish -f getip.yml --platforms linux/arm64"
                sh "killall dockerd"
            }
        }
    }
}

