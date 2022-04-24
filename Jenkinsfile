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
                sh "find / -name cli-plugins"
                // sh "curl -sLS https://github.com/docker/buildx/releases/download/v0.8.2/buildx-v0.8.2.linux-amd64 -o buildx"
                // sh "cp buildx /usr/libexec/docker/cli-plugins/docker-buildx"
                // sh "chmod +x /usr/libexec/docker/cli-plugins/docker-buildx"
                sh "dockerd &"
                sh "docker plugin install buildx"
                // sh "docker buildx install"
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

