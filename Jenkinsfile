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
        stage ("build & SonarQube analysis") {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtMavenDeployer (
                    id: "jfrog",
                    serverId: "swathi",
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MAVEN_TOOL, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "jfrog",
                    )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "swathi"
                )
            }
        }
    }
}