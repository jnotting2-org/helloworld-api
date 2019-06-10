pipeline {
  agent any
  stages {
    stage('Build') {
      when {
        beforeAgent true
        branch 'development'
      }
      steps {
        echo 'build'
        writeFile file: "application.sh", text: "echo Built ${BUILD_ID} of ${JOB_NAME}"
        archiveArtifacts 'application.sh'
        gateProducesArtifact file: 'application.sh', label: 'Dummy artifact to be consumed by Deploy (master branch) gate'
      }
    }
    stage('Test') {
      when {
        beforeAgent true
        branch 'test'
      }
      steps {
        copyArtifacts projectName: '../helloworld-api/development'
        gateConsumesArtifact file: 'application.sh'
      }
    }
    stage('Deploy') {
      when {
        beforeAgent true
        branch 'master'
      }
      steps {
        publishEvent simpleEvent('hello-api-eli')
        copyArtifacts projectName: '../helloworld-api/development'
        gateConsumesArtifact file: 'application.sh'
      }
    }
  }
}
