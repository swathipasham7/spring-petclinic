pipeline {
    agent {label 'spcpetclinic'}
    triggers {
        pollSCM ('15 * * * *')
    }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/swathipasham7/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('build') {
            steps {
                sh './mvnw package'
            }
        }
        stage("build & sonarqube") {
            steps {
              withSonarQubeEnv('My SonarQube Server') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
    }
}