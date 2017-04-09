#!/usr/bin/env groovy

// Get the project name from multibranch job name (doesn't work in other build types)
def projectName = env.JOB_NAME.split('/')[0]
def buildStatus = 'SUCCESS'

lock("lock_${projectName}_${env.BRANCH_NAME}") {
  stage('checkout') {
    checkout scm

    stash name: 'Gemfile', includes: 'Gemfile,Gemfile.lock'
  }

  stage('bundle') {
    withEnv(['RBENV_VERSION=2.2.4']) { sh 'bundle install --without production' }
  }

  stage('test') {
    withEnv(['RBENV_VERSION=2.2.4']) {
      def rspecResult = 0
      try {
        timeout(12) {
          rspecResult = sh(script: "bundle exec rake test test/rack_test.rb", returnStatus: true)
          println rspecResult
          if (rspecResult > 0) {
            error "build failed with exit code ${rspecResult}"
          }
        }
      } finally {
        step([$class: 'JUnitResultArchiver', testResults: '*.xml', allowEmptyResults: true])
      }
    }
  }
}

