pipeline {
  agent any
 environment {
   registry = 'gcr.io'
   appName = 'getintodevops-hellonode'
   projectName = 'dark-arcade-269117'
   dockerTag = "BUILD-${BUILD_NUMBER}"
 }

stages {
    stage('Clone repository') {
        steps {
          checkout scm
        }
    }

    stage('Build image') {
        steps {

          sh("gcloud auth configure-docker")
          sh("docker build  -t '${env.registry}'/'${env.projectName}'/'${env.appName}':${dockerTag} .")

        }
   }

    stage('Push image') {
         steps {
           withCredentials([file(credentialsId: 'google', variable: 'GC_KEY')]) {
             // sh("docker login '${env.registry}' -u artifactory-cicd-pipeline-user -p '${artifactoryCred}'")
             sh("cat ${GC_KEY} | docker login -u _json_key --password-stdin https://gcr.io")
             // sh("gcloud auth activate-service-account --key-file=${GC_KEY}")
             sh("docker -- push '${env.registry}'/'${env.projectName}'/'${env.appName}':${dockerTag}")
          }
         }
   }
}
}
