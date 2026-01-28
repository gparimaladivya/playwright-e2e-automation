pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Install deps') {
      steps {
        sh '''
          node -v || true
          npm -v  || true
          npm ci
        '''
      }
    }

    stage('Install Playwright browsers') {
      steps { sh 'npx playwright install --with-deps' }
    }

    stage('Run tests') {
      steps { sh 'npx playwright test' }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
      archiveArtifacts artifacts: 'test-results/**', allowEmptyArchive: true
    }
  }
}
