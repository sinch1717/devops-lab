pipeline {

  // Use Docker container as build agent
  agent {
    docker {
      image 'node:18-alpine'
      args  '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {

    stage('Install') {
      steps {
        sh 'node --version && npm ci'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }

  }

}

// Per-stage docker agents (different tools per stage)
pipeline {
  agent none

  stages {
    stage('Node Build') {
      agent { docker { image 'node:18-alpine' } }
      steps { sh 'npm ci && npm test' }
    }

    stage('Docker Push') {
      agent { docker { image 'docker:dind' } }
      steps { sh 'docker build -t myapp:latest .' }
    }
  }
}
