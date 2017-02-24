#!/usr/bin/env groovy
stage('bundle') {
  withEnv(['RBENV_VERSION=2.2.4']) { sh 'bundle install --without production' }
}

stage('test') {
  withEnv(['RBENV_VERSION=2.2.4']) {
    sh 'bundle exec rake test'
    step([$class: 'JUnitResultArchiver', testResults: '*.xml', allowEmptyResults: true])
  }
}

