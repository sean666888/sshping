pipeline {
  agent any
  stages {
    stage('debian') {
      steps {
        sh 'mkdir $WORKSPACE/build'
        cmake(installation: 'InSearchPath', workingDir: 'build', arguments: '..')
        sh '''cd build
make'''
      }
    }
    stage('package') {
      steps {
        cpack(installation: 'InSearchPath', workingDir: 'build')
      }
    }
    stage('error') {
      steps {
        archiveArtifacts 'build/*.deb'
      }
    }
  }
  environment {
    GITHUB_TOKEN = credentials('git-pat')
  }
}