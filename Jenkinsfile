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
        sh '/var/lib/jenkins/go/bin/github-release release --name $GIT_TAG_NAME --description "TO DO" --user sean666888 --repo sshping --tag v0.1.5'
        sh '''cd build
/var/lib/jenkins/go/bin/github-release upload --name "sshping-debian-amd64" --file *.deb --user sean666888 --repo sshping --tag v0.1.5'''
        archiveArtifacts 'build/*.deb'
      }
    }
  }
  environment {
    GITHUB_TOKEN = credentials('git-pat')
  }
}