@Library('jenkins-devops@dev')_ // load jenkins-devops as a library

properties([
  parameters([
    choice(      name: 'TYPE',              choices:     ['converge', 
                                                          'dismiss'],
                                            defaultValue: 'converge',       description: 'TYPE of run Deploy or Dismis packages'),
    string(      name: 'SERVICE',           defaultValue: 'default',        description: 'Name of Service (also namespace too)')
  ])
])

pipeline {

  agent {
    label 'multiwerf'
  }

  environment {
    WERF_TMP_DIR                = '/home/jenkins/tmp/'
    WERF_HOME                   = '/home/jenkins/tmp/.werf/'

    YC_AUTH                     = credentials("ya_registry_key")
    KUBECONFIG                  = credentials("werf_kube_config")
    WERF_SECRET_KEY             = credentials("WERF_SECRET_KEY")
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage("Prerequisites") {
      steps {
        script {
          sh 'export'
        }
      }
    }
    stage("DEV") {
      when {
        anyOf { branch 'dev'; }
      }
      environment { 
        WERF_NAMESPACE          = "${params.SERVICE}"
        WERF_ENV                = "dev"
        WERF_KUBE_CONTEXT       = "dev"
      }
      steps {
        script {
          functions.runWerf("${params.TYPE}")
        }
      }
    }
    stage("PROD YC") {
      when {
        anyOf { branch 'master'; }
      }
      environment { 
        WERF_NAMESPACE          = "${params.SERVICE}"
        WERF_ENV                = "production"
        WERF_KUBE_CONTEXT       = "production"
      }
      steps {
        script {
          functions.runWerf("${params.TYPE}")
        }
      }
    }
    stage("PROD AWS") {
      when {
        anyOf { branch 'master'; }
      }
      environment { 
        AWS_ACCESS_KEY_ID       = credentials('aws_access_key_id')
        AWS_SECRET_ACCESS_KEY   = credentials('aws_secret_access_key')

        WERF_NAMESPACE          = "${params.SERVICE}"
        WERF_ENV                = "aws-production"
        WERF_KUBE_CONTEXT       = "aws-production"
      }
      steps {
        script {
          functions.runWerf("${params.TYPE}")
        }
      }
    }
  }
}
