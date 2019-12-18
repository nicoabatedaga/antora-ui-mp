#!/bin/env groovy

def githubApiTokenCredentialsId = 'docs-robot-api-key'

def githubApiTokenCredentials = string(credentialsId: githubApiTokenCredentialsId, variable: 'GITHUB_API_TOKEN')

pipeline {
  agent any
  options {
    disableConcurrentBuilds()
  }
  stages {
    stage('Configure') {
      steps {
        script {
          properties([
            [$class: 'GithubProjectProperty', projectUrlStr: 'https://github.com/nicoabatedaga/antora-ui-mp'],
            pipelineTriggers([githubPush()]),
          ])
        }
      }
    }
    stage('Install') {
      steps {
        nodejs('node10') {
          sh 'npm install --quiet --no-progress --cache=.cache/npm --no-audit'
        }
      }
    }
    stage('Release') {
      steps {
        dir('public') {
          deleteDir()
        }
        withCredentials([githubApiTokenCredentials]) {
          nodejs('node10') {
            sh '$(npm bin)/gulp release'
          }
        }
      }
    }
  }
}
