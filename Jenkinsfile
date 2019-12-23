@Library('openshift-library') _

openshift.withCluster() {
  env.APP_NAME = "springboot-rest-example"
  env.SBX_ENV = openshift.project()
  env.ARTIFACT_DIRECTORY = "openjdk/spring-boot-rest-example/build/libs"
}


def gradleCmd = './gradlew'

pipeline {

  agent { label 'maven' }

  stages {

    stage('SCM Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh "cd openjdk/spring-boot-rest-example && ${gradleCmd} clean build -xtest"
      }
    }

    stage("Binary Build") {
        steps{
            binaryBuild(projectName: "${SBX_ENV}", buildConfigName: "${APP_NAME}", artifactsDirectoryName: "${ARTIFACT_DIRECTORY}")
        }
    }

    stage ("Verify Deployment") {
        steps {
            verifyDeployment(projectName: "${SBX_ENV}", targetApp: "${APP_NAME}")
        }
    }
  }
}
