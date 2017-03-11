#!/usr/bin/env groovy
stage('bundle') {
  withEnv(['RBENV_VERSION=2.2.4']) { sh 'bundle install --without production' }
}

stage('test') {
  withEnv(['RBENV_VERSION=2.2.4']) {
    def rspecResult = 0
    try {
      timeout(12) {
        rspecResult = sh(script: "bundle exec rake test", returnStatus: true)
        if (rspecResult > 0) {
          error "build failed with exit code ${rspecResult}"
        }
      }
    } finally {
      step([$class: 'JUnitResultArchiver', testResults: '*.xml', allowEmptyResults: true])
    }
  }
}

