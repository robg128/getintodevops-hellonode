node {
    def app
//    dockerTag = "${GIT_COMMIT}" + "-BUILD${BUILD_NUMBER}"
 environment {
   registry = 'monsternext-jobs-docker-registry-local.jfrog.io'
   appName = 'jobs-sam-indexer'
 }

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
sh("echo ${env.registry}'")
 sh("gcloud auth configure-docker")

// sh("docker build --build-arg maven_settings=maven_settings.xml --network=host -t '${env.registry}'/'${env.appName}':${dockerTag} .")
sh("docker build -t gcr.io/dark-arcade-269117/getintodevops-hellonode .")

//        app = docker.build("gcr.io/dark-arcade-269117/getintodevops-hellonode")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
withCredentials([file(credentialsId: 'google', variable: 'GC_KEY')]) {
    sh("gcloud auth activate-service-account --key-file=${GC_KEY}")
    sh("docker -- push gcr.io/dark-arcade-269117/getintodevops-hellonode:latest")
  }
    }
}
