def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label,
containers: [
    containerTemplate(privileged: true, name: 'dind', image: 'docker:dind', ttyEnabled: true, command: 'cat')
  ]) {
    node(label) {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/cwdrunner/faasdemo.git']]])
	    
        stage("Docker info")  {
            container("dind") {
		 withCredentials([usernamePassword(credentialsId: 'docker-cwdrunner', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
  		// available as an env variable, but will be masked if you try to print it out any which way
  		// note: single quotes prevent Groovy interpolation; expansion is by Bourne Shell, which is what you want
  		sh 'echo $DOCKER_PASSWORD'
  		// also available as a Groovy variable
  		echo DOCKER_USER
  		// or inside double quotes for string interpolation
 		echo "username is $DOCKER_USER"
		sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin"
			 
		// Other stuff missing
		sh "apk add unzip"
	
		// Start man install
                sh "apk add --no-cache curl git && curl -sLS cli.openfaas.com | sh"
                // buildx is needed for multi-arch builds. Some mages have it but not this one
                sh "mkdir -p ~/.docker/cli-plugins"
                sh "curl -sLS https://github.com/docker/buildx/releases/download/v0.8.2/buildx-v0.8.2.linux-amd64 -o ~/.docker/cli-plugins/docker-buildx"
                sh "chmod +x ~/.docker/cli-plugins/docker-buildx"
                sh "dockerd &"
                sh "docker buildx install"
	 	// This may or may not be needed.
                // sh "docker run --rm --privileged multiarch/qemu-user-static --reset -p yes"
                sh "export DOCKER_CLI_EXPERIMENTAL=enabled"
                // Needed for faas-cli conversion
                sh "export DOCKER_USER='cwdrunner'"
		sh "echo docker user is $DOCKER_USER"
                sh "docker info"
                sh "faas-cli template store pull java11"
		// DOCKER_USER is used in getip.yml
                sh "faas-cli publish -f getip.yml --platforms linux/arm64"
                sh "killall dockerd"
		 }
            }
        }
    }
}

