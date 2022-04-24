def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label,
containers: [
    containerTemplate(privileged: true, name: 'dind', image: 'docker:dind', ttyEnabled: true, command: 'cat')
  ]) {
    node(label) {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/cwdrunner/faasdemo.git']]])
        environment {
		    DOCKERHUB_CREDENTIALS=credentials('docker-cwdrunner')
	       }
        stage("Docker info")  {
            container("dind") {
		sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "apk add --no-cache curl git && curl -sLS cli.openfaas.com | sh"
                // buildx is needed for multi-arch builds. Some mages have it but not this one
                sh "mkdir ~/.docker ~/.docker/cli-plugins"
                sh "curl -sLS https://github.com/docker/buildx/releases/download/v0.8.2/buildx-v0.8.2.linux-amd64 -o ~/.docker/cli-plugins/docker-buildx"
                sh "chmod +x ~/.docker/cli-plugins/docker-buildx"
                sh "dockerd &"
                sh "docker buildx install"
                sh "docker run --rm --privileged multiarch/qemu-user-static --reset -p yes"
                sh "export DOCKER_CLI_EXPERIMENTAL=enabled"
                // Needed for faas-cli conversion
                sh "export DOCKER_USER=cwdrunner"
                sh "docker info"
                sh "faas-cli template store pull java11"
                sh "faas-cli publish -f getip.yml --platforms linux/arm64"
                sh "killall dockerd"
            }
        }
    }
}

