pipeline {
  agent any
 environment {
   registry = 'gcr.io'
   appName = 'getintodevops-hellonode'
   projectName = 'dark-arcade-269117'
   dockerTag = "-BUILD${BUILD_NUMBER}"
 }

stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
         steps {

        checkout scm
}
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
      steps {

 sh("gcloud auth configure-docker")

// sh("docker build  -t '${env.registry}'/'${env.projectName}'/'${env.appName}':${dockerTag} .")
// sh("docker build -t '${env.registry}'/dark-arcade-269117/getintodevops-hellonode .")

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
    sh("docker -- push '${env.registry}'/'${env.projectName}'/'${env.appName}':${dockerTag}")
    //sh("docker -- push gcr.io/dark-arcade-269117/getintodevops-hellonode:latest")
}
  }
    }
}
}
