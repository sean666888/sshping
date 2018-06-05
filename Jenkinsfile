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
        sh '''github-release upload \\
    --user sean666888 \\
    --repo sshping \\
    --tag $GIT_TAG_NAME \\
    --name "sshping-debian-amd64" \\
    --file build/*.deb'''
      }
    }
  }
  environment {
    GITHUB_TOKEN = credentials('jenkins-git-pat')
  }
}