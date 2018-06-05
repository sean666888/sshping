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
        sh '## /var/lib/jenkins/go/bin/github-release release --user sean666888 --repo sshping --tag $GIT_TAG_NAME --name $GIT_TAG_NAME --description "TO DO"'
        sh '''cd build
/var/lib/jenkins/go/bin/github-release upload --user sean666888 --repo sshping --tag $GIT_TAG_NAME --name "sshping-debian-amd64" --file *.deb'''
        archiveArtifacts 'build/*.deb'
      }
    }
  }
  environment {
    GITHUB_TOKEN = credentials('git-pat')
  }
}