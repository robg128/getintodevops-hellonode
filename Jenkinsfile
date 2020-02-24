pipeline {
  agent any
 environment {
   registry = 'gcr.io'
   appName = 'jobs-sam-indexer'
   dockerTag = "-BUILD${BUILD_NUMBER}"
 }

stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
         steps {

        checkout scm
        //shortCommit = sh returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
sh("echo '${dockerTag}'")
}
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
      steps {

 sh("gcloud auth configure-docker")

// sh("docker build --build-arg maven_settings=maven_settings.xml --network=host -t '${env.registry}'/'${env.appName}':${dockerTag} .")
sh("docker build -t '${env.registry}'/dark-arcade-269117/getintodevops-hellonode .")

//        app = docker.build("gcr.io/dark-arcade-269117/getintodevops-hellonode")
    }
}

    stage('Push image') {
         steps {
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
}
}
