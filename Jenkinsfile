pipeline {
 agent {
  kubernetes {
   label 'slave-docker'
   defaultContainer 'docker'
  }
 }
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
container('maven') {
		echo("Inside container")
	        } //container

        app = docker.build("gcr.io/docker-demo-268422/getintodevops-hellonode")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://gcr.io', 'gcr:google-container-registry-project') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
